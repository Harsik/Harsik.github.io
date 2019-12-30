---
layout: post
title: "SpringBoot 프로젝트 시작하기 15 - 아바타칸 만들기"
date: 2019-11-04
categories: SpringBoot
tags: Vuetify
---
<div style="display:none;">
아바타 
전체적인 ui 개선
제목은 뭘로 할까? 전체적인 ui 개선이라는 개요를 만들긴 했지만 막상 할려고 하니 잘 안되네 일단 ui 개선에 필수 적인 부분을 꼽자면 역시 그리드이다. 아니 먼저 아바타 부터
</div>
<h3>아바타</h3>
언젠가부터였을까? 사람들이 프로필 사진을 대신할 말로 아바타를 쓰기 시작했던 때는. 깃허브에 가입하고 나서 프로파일을 적을려고 했을 때 사진을 넣을만한 공간에 아바타를 넣으라는 문구가 적혀있었다. 그외의 다른 플랫폼 또한 아바타라는 말을 사용하고 있다. 아바타라는 말을 사용하는 것이 트렌드가 되어버린 것 같다. 그래서 필자도 그 흐름에 따라 볼려고 한다. 
<br><br>
파일 서버 구축 포스트에서도 언급했다시피 프로필 사진을 넣는다고 하였다. 하지만 어떻게 넣을 것인가에서는 얘기하지 않았으니 지금부터 생각해보자. 당장 파일을 불러 넣는다고 해도 그 이후는 어떻게 할 것인가. 우리는 파일 테이블을 만들었으니 파일 테이블에 아바타 파일로 사용하기 바라는 파일에 유저 아이디 속성을 붙이는 방법도 있지만 대부분의 파일은 아바타 파일이 아닐 것이기에 그다지 좋은 방법이라고 할 수 없다. 
<br><br>
그렇다면 반대로 유저에게 파일 아이디를 달아야 하는가? 하지만 유저들이 아바타를 항상 넣는 것도 아니기 때문에 이 또한 좋은 방법이 아니다. 그렇다면 다대다 관계로 유저테이블과 파일테이블을 연결하면 되!라고 생각 할 수도 있다. 하지만 필자는 유저가 여러개의 아바타를 넣는 것이 아니기에 이 또한 좋은 방법이라고 생각하지 않는다.
<br><br>
그래서 아바타 파일 정보를 저장할 테이블을 하나 만들기로 하였다. 프로파일과 일대일 관계로 만들면 유저와 일대일 관계도 해결 된다. 유저테이블이 아닌 프로파일 테이블과 일대일 관계로 만든 건 프로파일 안에 아바타 업로드가 있어서 그렇게 만들었다. 더불어 아직 한 테이블에 두 가지 관계를 넣는 것을 해보지 않았기 때문이기도 하다. 아바타 파일을 업로드 할 시 기존의 업로드 방식을 사용하면서 아바타 테이블에 파일 정보를 넣는다면 파일 리스트에서 나오지 않으면서 아바타 파일은 적용된다. 
<br><br>
글이 너무 길어지니 바로 파일 클래스를 만드는 것 처럼 아바타 클래스를 만들면서 프로파일과 일대일 매핑 시켜보자.

<h4>AvatarFileInfo.java</h4>
```java
@Entity
@Getter
@Setter
public class AvatarFileInfo {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String downloadUri;

    private String type;

    private Long size;

    @OneToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "profile_id", nullable = false)
    @JsonIgnore
    private Profile profile;

    @CreationTimestamp
    private LocalDateTime createdAt;

    @UpdateTimestamp
    private LocalDateTime updatedAt;

    public AvatarFileInfo() {
        this.name = "default";
        this.downloadUri = "http://localhost:8080/api/file/downloadFile/512x512.png";
        this.type = "image/png";
        this.size = new Long("20001");
    }

    @Builder
    public AvatarFileInfo(String name, String downloadUri, String type, Long size) {
        this.name = name;
        this.downloadUri = downloadUri;
        this.type = type;
        this.size = size;
    }
}

```
<br><br>

<h4>FileController.java</h4>
```java
@CrossOrigin
@RestController
@RequestMapping("/api/file")
public class FileController {
    @GetMapping("/loadAvatar")
    @PreAuthorize("hasRole('USER')")
    public AvatarFileInfo loadAvatar(@RequestParam(value = "email") String email) {
        return accountService.loadAvatarByEmail(email);
    }

    @PostMapping("/uploadAvatar")
    public UploadFileResponse uploadAvatar(@RequestParam("file") MultipartFile file,
            @RequestParam("email") String email) {
        String fileName = fileStorageService.storeFile(file);
        String fileDownloadUri = ServletUriComponentsBuilder.fromCurrentContextPath().path("/api/file/downloadFile/")
                .path(fileName).toUriString();

        accountService.saveAvatar(email, fileName, fileDownloadUri, file.getContentType(), file.getSize());
        return new UploadFileResponse(fileName, fileDownloadUri, file.getContentType(), file.getSize());
    }
}

```
<br><br>

<h4>Profile.java</h4>
```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@Table(name = "profiles")
public class Profile {
    @OneToOne(fetch = FetchType.LAZY, cascade = CascadeType.ALL, mappedBy = "profile")
    private AvatarFileInfo avatarFileInfo;

}

```
<br><br>

<h4>AvatarFileInfoRepository.java</h4>
```java
public interface AvatarFileInfoRepository extends JpaRepository<AvatarFileInfo, String> {
    Optional<AvatarFileInfo> findByProfileId(Long ProfileId);
    Boolean existsByProfileId(Long ProfileId);
}

```
<br><br>
<h4>AccountService.java</h4>
```java
@Service
public class AccountService implements UserDetailsService {
  
        @Autowired
        private AvatarFileInfoRepository avatarFileInfoRepository;

        public AvatarFileInfo loadAvatarByEmail(String email) {
                Account account = accountRepository.findByEmail(email).orElseThrow(
                                () -> new UsernameNotFoundException("Account not found with email : " + email));
                Profile profile = account.getProfile();
                return avatarFileInfoRepository.findByProfileId(profile.getId())
                                .orElseThrow(() -> new UsernameNotFoundException(
                                                "avatarFileInfo not found with profile_id : " + profile.getId()));
        }

        public void saveAvatar(String email, String name, String downloadUri, String type, Long size) {
                Account account = accountRepository.findByEmail(email).orElseThrow(
                                () -> new UsernameNotFoundException("Account not found with email : " + email));
                Profile profile = account.getProfile();
                AvatarFileInfo avatarFileInfo = profile.getAvatarFileInfo(); 

                if (avatarFileInfo != null) {
                        avatarFileInfo.setName(name);
                        avatarFileInfo.setDownloadUri(downloadUri);
                        avatarFileInfo.setType(type);
                        avatarFileInfo.setSize(size);
                        avatarFileInfoRepository.save(avatarFileInfo);
                } else {
                        AvatarFileInfo newAvatar = new AvatarFileInfo(name, downloadUri, type, size);
                        profile.setAvatarFileInfo(newAvatar);
                        newAvatar.setProfile(profile);
                        profileRepository.save(profile);
                }
        }
}
```
<br><br>

이제까지 해왔던 것들을 적용시키는 것이기에 부가적인 설명은 하지 않겠다. 조금 새로운 것이 있다면 사용자의 아이디인 이메일로 프로파일을 찾는 것이기 때문에 클래스를 만들때 위와 같은 코드를 갖게 된다는 것이다. 업로드시 페이로드는 파일업로드시 사용했던 것으로 그대로 사용할 것이다. 여기서 포스트맨으로 아바타가 정상적으로 업로드가 되는지 확인해보자.

<br><br>
![uploadAvatar](/files/file/uploadAvatar.png)
<br><br>
![loadAvatar](/files/file/loadAvatar.png)
<br><br>

/loadAvatar 요청시 이전에 했던 uploadAvatar Body를 비워야 에러가 생기지 않으니 주의바란다. 둘다 별 문제 없이 된다면 우리는 화면을 만들 준비가 된 것이다. 화면을 어떻게 만들까?
<br><br>
![planAvatar](/files/planAvatar.png)
<br><br>
기존의 프로파일 정보를 표시해 주는 것에 연장해서 넣기에는 너무 위아래로 늘려지는 것 같아 왼쪽에 아바타 사진을 놓으려고 한다. 그렇게 하기 위해서 이미지를 보여지게할 방법이 필요한데 html에서도 이미지를 표시하는 여러 방법이 있는 것처럼 Vuetify 또한 이미지를 표시하는 <b><a href="https://vuetifyjs.com/en/components/images">v-avatar</a></b>컴포넌트를 제공한다. 이 컴포넌트와 v-card 컴포넌트를 활용하여 계획처럼 되도록 만들어보자.

<br><br>
<h4>Profile.vue</h4>
```javascript
<template>
  <v-layout align-start justify-center row wrap>
    <v-flex xs12 sm6 md4>
      <v-card class="pa-3 ma-1">
        <div class="headline">
          <v-layout align-center justify-start>{{ avatarText }}</v-layout>
          <v-divider></v-divider>
          <v-layout class="pa-3" align-center justify-center>
            <v-avatar :tile="true" :size="300" color="grey lighten-4">
              <img :src="imageUrl" alt="avatar">
            </v-avatar>
          </v-layout>
          <v-btn raised class="primary" @click="onPickFile">Upload</v-btn>
          <input
            type="file"
            style="display: none"
            ref="fileInput"
            accept="image/*"
            @change="onFilePicked"
          >
        </div>
      </v-card>
    </v-flex>
    </v-layout>
</template>
```

<br><br>
![avatarProfile](/files/avatarProfile.png)
<br><br>
화면을 만들었으니 이제 기능을 넣을 차례이다. 사진을 업로드하면 아바타의 사진이 본 avatar구간에 보여져야하며 파일이 업로드되고 자료가 기록되어야 한다. 또한 사진의 url은 서버의 url이여야 하기때문에 업로드 후 바로 사진을 불러와야한다. 불러오는 기능은 처음 프로파일이 화면에 뜰 때 또한 작동해야한다. 그러면 위 기능을 작성해보자. 이전 파일을 다루는 포스트들에서 적용했던 코드들을 응용하여 적용시켜야한다.
<br><br>
```javascript
<script>
import { uploadAvatar, loadAvatar } from './APIUtils'
export default {
  data: () => ({
    imageUrl: '',
    avatarText: 'Avatar'
  }),
  mounted () {
    this.onLoadAvatar()
  },
  methods: {
    onLoadAvatar () {
      loadAvatar(this.email)
        .then(response => {
          this.imageUrl = response.downloadUri
        })
        .catch(error => {
          /* eslint-disable no-console */
          console.log(error)
        })
    },
    onUploadAvatar (file) {
      let formData = new FormData()
      formData.append('file', file)
      formData.append('email', this.email)
      uploadAvatar(formData)
        .then(() => {
          this.onLoadAvatar()
        })
        .catch(error => {
          /* eslint-disable no-console */
          console.log(error)
        })
    },
    onFilePicked (event) {
      const files = event.target.files // file info load
      let filename = files[0].name
      if (filename.lastIndexOf('.') <= 0) { // filename 유효성 검사
        return alert('Please pick valid file')
      }
      const fileReader = new FileReader()
      fileReader.addEventListener('load', () => {
        this.imageUrl = fileReader.result
      })
      fileReader.readAsDataURL(files[0])
      this.onUploadAvatar(files[0])
    },
    onPickFile () {
      this.$refs.fileInput.click()
    }
  }
}
</script>
```
<br><br>
<h4>APIUtill.js</h4>
```javascript

export function loadAvatar (email) {
  return request({
    url: API_BASE_URL + '/file/loadAvatar?email=' + email,
    method: 'GET'
  })
}

export function uploadAvatar (formData) {
  return formRequest({
    url: API_BASE_URL + '/file/uploadAvatar',
    method: 'POST',
    body: formData
  })
}

```
<br><br>
이렇게 하면 로그인을 하여 로컬스토리지에 저장되어 있는 아이디 값을 키값으로 프로파일 정보와 아바타 정보를 불러올 수 있다. 업로드와 로드는 서로 통신 메소드도 사용하는 요청방식도 다르니 주의해야한다. 
<br><br>
![uploadDear](/files/file/uploadDear.png)
<br><br>
![uploadDear2](/files/file/uploadDear2.png)
<br><br>
위와 같이 업로드한 사진을 아바타 사진으로 사용할 수 있는 것을 볼 수 있다. 아바타 사진을 올리기 위해 파일서버부터 다운로드까지의 장문의 포스트를 걸쳐서 여기까지 왔다. 이 다음 서버 기능이 아니라 화면에 집중하기 위해 Vuetify에 여러 기능들을 소개하고 실제 적용하여 현 앱을 좀 더 보기 좋게 꾸미는 작업을 할 것이다.