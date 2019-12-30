---
layout: post
title: "SpringBoot 프로젝트 시작하기 14 - 파일 업로드, 다운로드"
date: 2019-10-10
categories: SpringBoot
tags: Vuetify
---
<div style="display:none;">
파일 업로드
파일 다운로드, 멀티다운로드
</div>
<hr class="divider">
<h3>파일 업로드</h3>
<br>
파일 업로드 하는 방법에는 기존에 사용하던 input태그를 사용해도 되지만 정말 볼품 없는 모습으로 아래의 사진처럼 보이게 된다.

<br><br>
![fileupload1](/files/file/fileupload1.png)
<br><br>

깔끔하게 짜여진 디자인 속에서 쌩뚱맞은 모습이다. 우리는 절대 이대로 사용할 수 없다. 그렇기에 vue의 ref 속성을 이용하여 input과 버튼을 연결시킬 것이다. 우선 <b><a href="https://kr.vuejs.org/v2/guide/components.html">ref 속성</a></b>에 대한 이해가 필요하다. 
<br><br>
링크에서도 나와있다시피 이 ref 속성은 자식 컴포넌트를 직접 접근하기 위해 사용하는 것으로 되어 있다. 하지만 컴포넌트를 보는 관점을 넓혀 현 Vue객체 this를 부모 컴포넌트로 본다면 안에 있는 input을 비롯한 다른 태그들도 자식 컴포넌트로 볼 수 있다. 그렇다면 아래와 같은 방법으로 위 문제를 해결 할 수 있다.
<br><br>
<h4>FileList.vue</h4>
```javascript
<template>
  <v-app>
    <v-btn raised class="primary" @click="onPickFile">Upload</v-btn>
        <input
          type="file"
          style="display: none"
          ref="fileInput"
          accept="*"
          required
        >
  </v-app>
</template>

<script>
export default {
  methods: {
    onPickFile () {
      this.$refs.fileInput.click()
    }
  }
}
</script>
```
<br><br>

v-btn을 클릭하면 아래 숨겨놓은 input을 클릭하는 것과 같도록 만들었다. 다음은 우리가 파일 서버에서 만들었던 것을 직접 활용할 때가 왔다. 파일 업로드 메소드를 만들 것이다. 포스트맨으로 했던 파일 업로드가 어떻게 앱에서 만들어 지는지 보자.

<hr class="divider">
<h3>폼 리퀘스트</h3>
<br>
<br><br>
<h4>APIUtill.js</h4>
```javascript

const formRequest = options => {
	const headers = new Headers({
	})

	if (localStorage.accessToken !== 'null') {
		headers.append('Authorization', 'Bearer ' + localStorage.accessToken)
	}

	const defaults = {
		headers: headers
	}
	options = Object.assign({}, defaults, options)

	// eslint-disable-next-line no-unused-vars
	return fetch(options.url, options).then(response => {})
}

export function uploadFile(formData) {
	return formRequest({
		url: API_BASE_URL + '/file/uploadFile',
		method: 'POST',
		body: formData
	})
}

```
<br><br>

기존에 사용하던 Request는 제이슨 객체를 보내는 것으로 헤더에 컨텐츠 타입을 맞추어 주었으나 이번 Request는 form을 보내는 것이다. 따라서 컨텐츠 타입 또한 바꾸어야 한다. 하지만 form은 따로 기입하지 않으면 자동으로 컨텐츠 타입이 정해지기 때문에 헤더에 추가할 필요가 없다. 
<br><br>

<h4>FileList.vue</h4>
```javascript
<template>
  <v-app>
    <v-btn raised class="primary" @click="onPickFile">Upload</v-btn>
        <input
          type="file"
          style="display: none"
          ref="fileInput"
          accept="*"
          @change="onFilePicked"
          required
        >
  </v-app>
</template>

<script>
import { uploadFile } from './APIUtils'
export default {
  methods: {
    onUploadFile (file) {
      let formData = new FormData()
      formData.append('file', file[0])
      uploadFile(formData)
        .then(() => {
          this.onLoadFiles()
        })
        .catch(error => {
          /* eslint-disable no-console */
          console.log(error)
        })
    },
    onFilePicked (event) {
      const files = event.target.files // file info load

      let filename = files[0].name
      if (filename.lastIndexOf('.') <= 0) {
        return alert('Please pick valid file')
      }
      const fileReader = new FileReader()
      fileReader.addEventListener('load', () => {
        this.imageUrl = fileReader.result
      })
      fileReader.readAsDataURL(files[0])
      this.onUploadFile(files)
    }
  }
}
</script>
```
<br><br>
<b><a href="https://developer.mozilla.org/ko/docs/Web/API/FileReaderfileReader">fileReader</a></b> API를 사용하여 클라이언트의 파일을 읽어 올 수 있다. 

<br><br>
![fileupload2](/files/file/fileupload2.png)
<br><br>

<hr class="divider">
<h3>멀티 업로드 기능 구현하기</h3>
<br>
일반 업로드가 성공했으니 멀티 업로드도 순조롭게 될 것이다. input 태그에 multiple 속성을 추가하여 한번에 여러개의 파일을 선택할 수 있도록 할 수 있다. 그렇게 하면 한번에 여러개의 파일을 올릴 수도 있고 하나만 선택해서 올릴 수도 있기에 필자는 업로드 메소드를 멀티 업로드 하나로 통합하여 쓸려고 한다. 

<br><br>
<h4>FileList.vue</h4>
```javascript
<template>
  <v-app>
    <v-btn raised class="primary" @click="onPickFile">Upload</v-btn>
        <input
          type="file"
          style="display: none"
          ref="fileInput"
          accept="*"
          @change="onFilePicked"
          multiple
          required
        >
  </v-app>
</template>

<script>
import { uploadFiles } from './APIUtils'
export default {
  methods: {
    onUploadFiles (files) {
      let formData = new FormData()
      for (let file in files) {
        formData.append('files', files[file])
      }
      uploadFiles(formData)
        .then(() => {
          this.onLoadFiles()
        })
        .catch(error => {
          console.log(error)
        })
    },
    onFilePicked (event) {
      const files = event.target.files // file info load

      let filename = files[0].name
      if (filename.lastIndexOf('.') <= 0) {
        return alert('Please pick valid file')
      }
      const fileReader = new FileReader()
      fileReader.addEventListener('load', () => {
        this.imageUrl = fileReader.result
      })
      fileReader.readAsDataURL(files[0])
      this.onUploadFiles(files)
    }
  }
}
</script>

```
<br><br>

<h4>APIUtill.js</h4>
```javascript

export function uploadFiles(formData) {
	return formRequest({
		url: API_BASE_URL + '/file/uploadMultipleFiles',
		method: 'POST',
		body: formData
	})
}

```
<br><br>
아래의 사진과 같이 한번에 여러개의 파일을 업로드 할 수 있다.
<br><br>
![fileupload3](/files/file/fileupload3.png)
<br><br>

<hr class="divider">
<h3>파일 다운로드 구현하기</h3>
<br>
다운로드 기능을 만드는데에도 고민이 많이 된다. 서버에 파일에는 어떻게 접근할 것인가. 내가 다운 받을려고하는 파일이 서버의 해당 파일이 맞는가. 서버는 일반적인 파일 시스템을 그대로 사용하기 때문에 같은 폴더 아래 같은 이름의 파일이 있으면 안된다. 그렇기에 파일마다 고유의 파일이름을 갖게 만들거나 아니면 파일의 이름을 기본키로 사용한다. 필자는 후자의 방법을 택해 같은 파일이 업로드 되면 기존의 같은 이름의 파일을 덮어씌워진다.
<br><br>
필자가 기존에 사용하던 방식으로 다운로드 링크를 넣어 클릭해 다운로드 하는 방식을 적용시킬려고 했으나 Vuetify의 업데이트로 Data tables 컴포넌트가 바뀌어 적용되지 않았다. 하는 수 없이 다른 방법을 찾아나서겠다. 그 다른 방법은  <b><a href="http://danml.com/download.html">downloadjs</a></b>를 사용하는 방법이다. downloadjs는 웹에서 다운로드 기능을 제공해 주는 라이브러리 파일로 다운 받고자 하는 URL을 입력하면 응답에 따라 파일로 다운로드 해준다. 우리는 이미 포스트맨으로 테스트를 해봤기 때문에 사용하기 충분한 조건을 갖추었다. 그렇다면 응용해보자.


<br><br>
<h4>FileList.vue</h4>
```javascript
<template>
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
        <v-btn raised class="primary" @click="onDownloadFiles">Download</v-btn>
</template>

<script>
let download = require('downloadjs');
export default {
  data: () => ({
    selected: []
    }),
  methods: {
    onDownloadFiles () {
      for (let value of this.selected) {
          download(value.downloadUri);
      }
    }
  }
}
</script>

```
<br><br>
datatable의 데이터를 저장할 select 배열 변수 선언과 checkbox를 사용하기 위한 show-select, item-key 속성을 추가하였다. 추가한 다운로드 버튼에 메소드를 바인딩 후 클릭한다면 파일이 다운로드 되는 것을 볼 수 있을 것이다.
<br><br>
![filedownload1](/files/file/filedownload1.png)
<br><br>





