---
layout: post
title: "SpringBoot 프로젝트 시작하기 13 - 파일리스트 만들기"
date: 2019-10-08
categories: SpringBoot
tags: Vuetify
comments: true
---
<div style="display:none;">
메뉴 버튼
네비게이션
네비게이션 메뉴
파일리스트 
파일리스트 그리드
파일 업로드
파일 다운로드, 멀티다운로드
</div>
<h3>네비게이션 서랍 만들기</h3>
<br>
&nbsp;지난 포스트에서 파일 서버를 구축하였습니다. 이제 이 파일 서버를 앱에서 나타내는 작업을 할 것입니다. 우리가 흔히 모바일 앱에서도 볼 수 있는 형태인 네비게이션 드로워, 간단히 말하자면 메뉴들이 담겨 있는 서랍을 하나 만들어 쓸 것입니다. 
<br><br>
&nbsp;Vuetify에서는 v-navigation-drawer라는 컴포넌트가 서랍기능을 제공합니다. 우리는 이것을 이용하여 새로운 메뉴를 만들어 파일 리스트 화면을 띄울 수 있게 만들 것입니다. 자세한 건 <b><a href="https://vuetifyjs.com/en/components/navigation-drawers#navigation-drawers">여기</a></b>를 참고하길 바라며 App 파일에 서랍기능을 추가해봅시다.

<br>
<h4>App.vue</h4>

```javascript
<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app>
      <v-list>
        <v-list-item :to="navMenu.to" :key="i" v-for="(navMenu, i) in navMenus">
          <v-list-item-content>
            <v-list-item-title v-text="navMenu.title"></v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar app>
      <v-app-bar-nav-icon @click.native.stop="drawer = !drawer" :disabled="!isAuthenticated"></v-app-bar-nav-icon>
      <v-toolbar-title class="headline text-uppercase">
        <span>Vuetify</span>
        <span class="font-weight-light">MATERIAL DESIGN</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items v-if="!isAuthenticated">
        <v-btn to="/Login">Login</v-btn>
        <v-btn to="/Signup">Signup</v-btn>
      </v-toolbar-items>
      <v-toolbar-items v-if="isAuthenticated">
        <v-layout align-center justify-space-between>
          <v-menu bottom left offset-y>
            <template v-slot:activator="{ on }">
              <v-btn v-on="on" icon>
                <v-icon large>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item v-for="(subMenu, i) in subMenus" :key="i" :to="subMenu.to">
                <v-list-item-title>{{ subMenu.title }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-layout>
      </v-toolbar-items>
    </v-app-bar>

    <v-content>
      <router-view @sendAuthentication="setAuthentication"></router-view>
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    drawer: false,
    isAuthenticated: true,
    subMenus: [
      { title: 'Profile', to: '/profile' },
      { title: 'Logout', to: '/logout' }
    ],
    navMenus: [
      { title: 'Welcome', to: '/' },
      { title: 'Inspire', to: '/Inspire' },
      { title: 'FileList', to: '/FileList' }
    ]
  }),
  methods: {
    setAuthentication (bool) {
      this.isAuthenticated = bool
    }
  }
}
</script>
```

<br>
&nbsp;v-app-bar에 app이라는 프로퍼티를 적용시켰기 때문에 v-navigation-drawer 또한 app 프로퍼티를 적용시켜야 합니다. app 프로퍼티는 응용 프로그램 레이아웃의 일부로 구성 요소를 지정시키는 것이기 때문에 선언하지 않았다면 선언한 컴포넌트 아래에 가려져 버립니다. 서랍을 열고 닫기를 할 수 있도록 상태 변수 하나를 추가시켰고, 서랍에 넣을 만한 것도 넣어봤습니다. 잘 되었다면 아래의 사진처럼 뜰 것입니다.

<br><br>
![drawer1](/files/file/drawer1.png)
<br><br>

&nbsp;아직 파일리스트 화면을 만들지 않았기 때문에 FileList를 클릭해도 아무일도 일어나지 않거나 홈화면으로 돌아갈 것입니다. 그럼 바로 파일리스트 화면을 만들어 보도록 합니다.

<br><br>

<hr class="divider">
<h3>파일리스트 만들기</h3>
<br>
&nbsp;저번 파일 서버 포스트에서 했던 것처럼 일들을 관리 하기 위해 데이터베이스에 Fileinfo 테이블을 만들고 저장했었습니다. 파일리스트 화면에서는 이를 그리드 화면을 구성하여 보여주는 것을 할려고 합니다. 뷰티파이에서도 그리드를 구성하기 쉽도록 컴포넌트를 제공하며 이름은 <b><a href="https://vuetifyjs.com/en/components/data-tables#api">v-data-table</a></b>이라고 합니다.

<br><br>
![datatable1](/files/file/datatable1.png)
<br><br>

&nbsp;위와 같이 만들려면 어떻게 해야 할까요? 우선 모양새부터 잡아보도록 합시다. 

<br>
<h4>FileList.vue</h4>

```javascript


```

<br>
![datatable2](/files/file/datatable2.png)
<br><br>

<h4>2019-12-21-mypost.md</h4>
{% raw %}
~~~javascript
<template>
  <v-layout align-start justify-center row wrap>
    <v-flex xs12 sm12 md12>
      <v-card class="pa-3">
        <v-data-table
          :headers="headers"
          :items="files"
          class="elevation-1"
        >
          <template v-slot:items="props">
            <td>{{ props.item.name }}</td>
            <td>{{ props.item.type }}</td>
            <td>{{ props.item.size }}</td>
            <td>{{ props.item.updatedAt }}</td>
          </template>
        </v-data-table>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  name: 'FileList',
  data: () => ({ 
    files: [
      {name:'name1',type:'type1',size:'100',updatedAt:'20191008'},
      {name:'name2',type:'type2',size:'100',updatedAt:'20191008'},
      {name:'name3',type:'type3',size:'100',updatedAt:'20191008'}
      ],
    headers: [
      {
        text: 'FileName',
        align: 'left',
        sortable: false,
        value: 'name'
      },
      { text: 'Type', value: 'type' },
      { text: 'Size', value: 'size' },
      { text: 'UpdatedAt', value: 'updatedAt' },
      { text: '', value: '' }
    ]
  }),
  mounted () {
  },
  methods: {
  }
}
</script>
~~~
{% endraw %}
<br>
&nbsp;간단한 모양새와 더미 데이터를 넣어보았습니다. 우리는 서버에서 응답을 받으면 위와 같은 Json 형태의 데이터를 받게 될 것입니다. 서버에 요청하고 응답을 받고 files 변수에 입력시키는 메소드를 만들면 파일리스트가 불러와지는 기능을 만들 수 있을 것입니다. 자 그렇다면 바로 만들어 봅시다.

<br>
<h4>APIUtills.js</h4>

```javascript
export function loadFiles() {
	return request({
		url: API_BASE_URL + '/file/loadFiles',
		method: 'POST'
	})
}
```

<br><br>
<h4>FileList.vue</h4>

```javascript
mounted () {
  this.onLoadFiles()
},
method:{
    onLoadFiles () {
      loadFiles()
        .then(response => {
          this.files = response
        })
        .catch(error => {
          /* eslint-disable no-console */
          console.log(error)
        })
    }
}
```

<br><br>
&nbsp;/* eslint-disable no-console */ 없이 console.log를 입력할려고 하면 에러가 뜨는 여러분들이 있을 것입니다. vscode에서 쓰는 eslint라는 유효성 검사 플러그인이 console.log를 사용하는 것을 막는 것인데 이 것뿐만 아니라 칸이 간격이라던가 쓰지 않는 변수에 대해서도 에러를 표시하여 불편함을 느낄 것입니다. eslint가 에러로 판단하고 메세지를 올리는 것인데 지금은 에러를 막는 것만 생각합시다. 
<br><br>
&nbsp;앱에서 요청이 끝났으니 서버에서도 응답과 서비스를 만들어야 합니다.

<br>
<h4>FileController.java</h4>

```java
    @PostMapping("/loadFiles")
    public List<FileInfo> loadFiles() {
        return fileStorageService.loadFiles();
    }
```

<br><br>
<h4>FileStorageService.java</h4>

```java
    public List<FileInfo> loadFiles() {
        return fileInfoRepository.findAll();
    }
```

<br><br>
&nbsp;서버를 실행하고 앱을 새로고침하면 아래와 같은 화면이 나올 것입니다.

<br>
![datatable3](/files/file/datatable3.png)
<br><br>

<hr class="divider">
<h3>파일 검색 기능 구현</h3>
<br>
&nbsp;우리가 저번 포스트에 했던 작업의 흔적이 남아있습니다. 그럼 이것 말고 또 뭘했었냐면 업로드 다운로드 기능을 구현하는 것도 하였습니다. 그 기능에 대한 요청 기능을 이 앱에서 추가시킬 것입니다. 하지만 두 기능을 이 글에 넣기엔 글이 너무 길어 질 것 같기에 검색 기능만 소개 및 추가하고 마무리하겠습니다. v-data-table 컴포넌트에서는 검색기능을 제공합니다. 검색 프로퍼티를 추가시키는 것은 물론 검색어 변수를 입력할 칸도 필요하기에 검색바와 겸사겸사 타이틀도 넣을 것입니다.


<br><br>
<h4>FileStorageService.java</h4>

```javascript
<template>
  <v-layout align-start justify-center row wrap>
    <v-flex xs12 sm12 md12>
      <v-card class="pa-3">
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
          :headers="headers"
          :items="files"
          :search="search"
          item-key="name"
          class="elevation-1"
        >
          <template v-slot:items="props">
            <td>{{ props.item.name }}</td>
            <td>{{ props.item.type }}</td>
            <td>{{ props.item.size }}</td>
            <td>{{ props.item.updatedAt }}</td>
          </template>
        </v-data-table>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
export default {
  name: 'FileList',
  data: () => ({
    search: ''
  })
}
```

<br><br>

&nbsp;여기서 item-key 프로퍼티가 찾고자하는 속성을 지정해줍니다. 파일명에서 찾을 것이기에 name으로 지정해 놓았습니다. 검색칸에 sunny라 치면 sunny로 시작하는 파일명을 가진 리스트를 보여줄 것입니다. 지금은 하나밖에 없기에 이것만 뜨지만 sunni로 입력하게 되면 바로 아래와 같이 맞는 자료를 찾을 수 없다고 뜰 것입니다.

<br>
![datatable5](/files/file/datatable5.png)
<br><br>











