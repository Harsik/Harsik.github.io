---
layout: post
title: "SpringBoot 프로젝트 시작하기 16 - UI 개선"
date: 2019-11-08
categories: SpringBoot
tags: Vuetify
---
<div style="display:none;">
전체적인 ui 개선
</div>
<h3>그리드, 레이아웃</h3>
여태까지 우리가 화면을 만들때는 기능을 최우선으로 생각하여 나머지에 대한 것은 적게 다뤘는데, 어느 정도 만들고자 하는 것을 다 만들었다고 생각하여 그 나머지에 대한것을 다루어 볼려고 한다. 메인화면도 그렇고 로그인, 로그아웃 화면이 프로파일이나 파일리스트 화면과 다르다. 브라우저의 창에 크기에 따라 커졌다가 작아졌다가 하는데 글간 간격도 일정하지도 않아 괜찮다고 생각하지 않는다.
<br><br>
그에 비해 프로파일이나 파일리스트 화면은 적절한 위치와 창 크기를 가지고 있어 꽤 괜찮아 보인다. 어떻게 그게 가능한 걸까? 코드에서 보면 알 수 있지만 그리드와 레이아웃 시스템 덕이다. 
<br><br>
<h3>그리드 시스템</h3>
그리드 시스템이 뭔지 뷰티파이 홈페이지의 설명을 빌리자면 뷰티파이는 12개의 포인트로 이루어져 있는 그리드 시스템을 사용하는 <b><a href="https://vuetifyjs.com/en/components/grids#grid-system">flex-box</a></b>라는 것을 가지고 있다. 이 그리드는 앱화면에 특수한 레이아웃을 형성하여 xs,sm,md,lg,xl라는 5가지 정지점을 기준으로 화면을 디자인한다.
<br><br>
![breakpoint](/files/vuetify/breakpoint.png)
<br><br>
요약하자면 뷰티파이는 그리드 시스템을 통해 반응성 웹을 만들 수 있다. 그래서 브라우저의 크기에 따라 앱 화면 크기를 조정할 수 있다. 그 다음은 레이아웃에 대한 설명이 필요하다.
<br><br>
<h3>레이아웃 시스템</h3>
글쓰다가 발견한 건데 6개월도 안되서 버전 업이후 레이아웃 시스템에 대한 설명이 사라졌다. 다행이 필자가 사용한 방법은 적용이 되어 앱화면은 문제가 없지만 설명할 도리가 없어졌다. 예전 버전쓰라고 하라고 할 수도 있겠지만 그렇게 하고 싶지는 않다.
<br><br>
![playground](/files/vuetify/playground.png)
<br><br>
잡설은 여기까지하고 전 레이아웃 시스템이 그리드 시스템에 편입되면서 설명이 사라졌다. 지금은 그리드 시스템-플레이 그라운드라는 이름으로 메뉴얼에 포함되어 있다. 설명을 적자면 해당 컴포넌트가 부모 컴포넌트 내에서 어떤 위치에 있을 지를 결정하고 형제 컴포넌트들과 어떤 정렬관계를 가지를 결정해주도록 도움을 주는 시스템이다. 그렇다면 바로 여태까지 만들었던 화면들을 그리드 시스템을 이용하여 적절하게 변경해보도록 하겠다.
<br><br>
<h4>HelloWorld.vue</h4>
```javascript
<template>
  <v-container>
    <v-row align="start" justify="center">
      <v-col xs="12" sm="6" md="6">
        <v-col xs="12">
          <v-img :src="require('../assets/logo.svg')" class="my-3" contain height="200"></v-img>
        </v-col>

        <v-col class="mb-4">
          <h1 class="display-2 font-weight-bold mb-3">Welcome to Vuetify</h1>
          <p class="subheading font-weight-regular">
            For help and collaboration with other Vuetify developers,
            <br />please join our online
            <a
              href="https://community.vuetifyjs.com"
              target="_blank"
            >Discord Community</a>
          </p>
        </v-col>

        <v-col class="mb-5" xs="12">
          <h2 class="headline font-weight-bold mb-3">What's next?</h2>

          <v-layout justify-center>
            <a
              v-for="(next, i) in whatsNext"
              :key="i"
              :href="next.href"
              class="subheading mx-3"
              target="_blank"
            >{{ next.text }}</a>
          </v-layout>
        </v-col>

        <v-col class="mb-5" xs="12">
          <h2 class="headline font-weight-bold mb-3">Important Links</h2>

          <v-layout justify-center>
            <a
              v-for="(link, i) in importantLinks"
              :key="i"
              :href="link.href"
              class="subheading mx-3"
              target="_blank"
            >{{ link.text }}</a>
          </v-layout>
        </v-col>

        <v-col class="mb-5" xs="12">
          <h2 class="headline font-weight-bold mb-3">Ecosystem</h2>

          <v-layout justify-center>
            <a
              v-for="(eco, i) in ecosystem"
              :key="i"
              :href="eco.href"
              class="subheading mx-3"
              target="_blank"
            >{{ eco.text }}</a>
          </v-layout>
        </v-col>
      </v-col>
    </v-row>
  </v-container>
</template>
```
<br><br>
![smHelloWorld](/files/vuetify/smHelloWorld.png)
<br><br>
<h4>InspireView.vue</h4>
```javascript
<template>
  <v-row align="center" justify="center">
    <v-col align="center">
        <img src="../assets/logo.svg" alt="Vuetify.js" class="mb-5" />
        <blockquote class="text-xs-center">
          &#8220;First, solve the problem. Then, write the code.&#8221;
          <footer>
            <small>
              <em>&mdash;John Johnson</em>
            </small>
          </footer>
        </blockquote>
    </v-col>
  </v-row>
</template>
```
<br><br>
![smInspireView](/files/vuetify/smInspireView.png)
<br><br>
지금은 사라진 v-layout과 v-flex 컴포넌트를 v-row와 v-col로 대체하였다. 내용이 적절한 위치에 있을 수 있도록 수정하였으며 화면의 크기에도 반응할 수 있도록 브레이크포인트를 정하였다. 모든 브레이크포인트에 대한 사진을 넣기에는 글이 너무 길어지기에 sm부분만 올리도록 하겠다.
로그인과 사인업 화면은 위 내용과 더불어 더 설명해야 할 것이 있기에 밑에서 따로 다루겠다.
<br><br>
<h3>스페이싱</h3>
<div style="display:none;">
우선 카드 와 텍스트 필드로 작성
스페이싱의 필요성 설명
스페이싱 설명
스페이싱 적용
</div>
지금 로그인 화면에는 앱 화면위에 텍스트 필드만 덩그라니 놓여 있는 상태이다. 여기서 조금 다듬어 보기위해 v-card를 넣어보도록 하겠다.
<br><br>
<h4>Login.vue</h4>
```javascript
<template>
  <v-form>
    <v-container>
      <v-row>
        <v-col xs="12" sm="6" md="6">
        <v-card>  
          <v-text-field v-model="email" :rules="emailRules" label="E-mail" required></v-text-field>
          <v-text-field
            v-model="password"
            :rules="passwordRules"
            :counter="8"
            type="password"
            label="Password"
            required
          ></v-text-field>
          <v-btn @click="onLogin">Login</v-btn>
        </v-card>
        </v-col>     
      </v-row>
    </v-container>
  </v-form>
</template>
```
<br><br>
![vcardLogin](/files/vuetify/vcardLogin.png)
<br><br>
화면을 보고 아량이 넓다면 이 화면도 좋게 넘어갈 수 있는 독자도 있을 수 있다. 하지만 필자는 여기서 좀 더 보기 좋은 화면으로 만들어 볼려고 한다.
<br><br>
![paddingMargin](/files/vuetify/paddingMargin.png)
<br><br>
위 그림과 같이 영역객체의 테두리 공간으로 두가지로 나누어 사용한다. 
<br><br>
![pa2](/files/vuetify/pa2.png)
<br><br>
pa-2를 해석하자면 패딩을 전방향에 2크기만큼 적용이라는 뜻이다. 그리고 v-divider라는 컨텐츠 사이를 나눌 때 쓰면 좋은 컴포넌트가 있다. 이것과 그리드시스템을 모두 적용 시키면 아래와 같다.
<br><br>
<h4>Login.vue</h4>
```javascript
<template>
  <v-form>
    <v-container>
      <v-row justify="center">
        <v-col xs="12" sm="6" md="6">
          <v-card class="pa-3">
            <v-subheader>Login</v-subheader>
            <v-divider :inset="false"></v-divider>
            <v-text-field v-model="email" :rules="emailRules" label="E-mail" required></v-text-field>
            <v-text-field
              v-model="password"
              :rules="passwordRules"
              :counter="8"
              type="password"
              label="Password"
              required
            ></v-text-field>
            <v-btn @click="onLogin">Login</v-btn>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>
```
<br><br>
![pa2CenterLogin](/files/vuetify/pa2CenterLogin.png)
<br><br>

<br><br>
<h4>Signup.vue</h4>
```javascript
<template>
	<v-form>
		<v-container>
			<v-row justify="center">
				<v-col xs="12" sm="6" md="6">
					<v-card class="pa-3">
						<v-subheader>Signup</v-subheader>
						<v-divider :inset="false"></v-divider>
						<v-text-field
							v-model="email"
							:rules="emailRules"
							label="E-mail"
							required
						></v-text-field>
						<v-text-field
							v-model="password"
							:rules="passwordRules"
							:counter="8"
							type="password"
							label="Password"
							required
						></v-text-field>
						<v-btn @click="onSignup">Signup</v-btn>
					</v-card>
				</v-col>
			</v-row>
		</v-container>
	</v-form>
</template>
```
<br><br>
![UISignup](/files/vuetify/UISignup.png)
<br><br>
<h4>Profile.vue</h4>
```javascript
<template>
	<v-container>
		<v-row justify="center">
			<v-col xs="12" sm="6" md="6">
				<v-card class="pa-3 ma-1">
					<div class="headline">
						<v-layout align-center justify-start>{{ avatarText }}</v-layout>
						<v-divider></v-divider>
						<v-layout class="pa-3" align-center justify-center>
							<v-avatar :tile="true" :size="300" color="grey lighten-4">
								<img :src="imageUrl" alt="avatar" />
							</v-avatar>
						</v-layout>
						<v-btn raised class="primary" @click="onPickFile">Upload</v-btn>
						<input
							type="file"
							style="display: none"
							ref="fileInput"
							accept="image/*"
							@change="onFilePicked"
						/>
					</div>
				</v-card>
			</v-col>

			<v-col xs="12" sm="6" md="6">
				<v-card class="pa-3 ma-1">
					<div class="headline">
						<v-layout align-center justify-start>{{ profileText }}</v-layout>
						<v-divider></v-divider>
					</div>
					<v-form class="pa-3" ref="form">
						<v-text-field
							label="Email"
							v-model="email"
							:disabled="true"
						></v-text-field>
						<v-text-field label="Name" v-model="profile.name"></v-text-field>
						<v-text-field label="Bio" v-model="profile.bio"></v-text-field>
						<v-text-field
							label="Company"
							v-model="profile.company"
						></v-text-field>
						<v-text-field
							label="Address"
							v-model="profile.address"
						></v-text-field>
						<v-btn color="primary" @click="onEditProfile">Edit</v-btn>
					</v-form>
				</v-card>
			</v-col>
		</v-row>
	</v-container>
</template>

```
<br><br>
![UIProfile](/files/vuetify/UIProfile.png)
<br><br>
<h4>FileList.vue</h4>
```javascript
<template>
	<v-container>
		<v-row justify="center">
			<v-col xs="12" sm="12" md="12">
				<v-card class="pa-3 ma-1">
					<v-toolbar flat color="white">
						<v-toolbar-title>FileList</v-toolbar-title>
					</v-toolbar>
					<v-card-title>
						<v-spacer></v-spacer>
						<v-text-field
							v-model="search"
							append-icon="mdi-magnify"
							label="Search"
							single-line
							hide-details
						></v-text-field>
					</v-card-title>
					<v-data-table
						v-model="selected"
						:headers="headers"
						:items="files"
						:search="search"
						show-select
						item-key="name"
						class="elevation-1"
					>
						</v-data-table>
					<v-btn raised class="primary" @click="onPickFile">Upload</v-btn>
					<input
						type="file"
						style="display: none"
						ref="fileInput"
						accept="*"
						@change="onFilePicked"
						multiple
						required
					/>
					<v-btn
						raised
						class="primary"
						@click="onDownloadFiles"
						:disabled="isDownloading"
						>Download</v-btn
					>
					</v-card>
			</v-col>
		</v-row>
	</v-container>
</template>
```
<br><br>
![UIFileList](/files/vuetify/UIFileList.png)
<br><br>

다른 화면들도 좋게 보이도록 바꾸어 보았다.