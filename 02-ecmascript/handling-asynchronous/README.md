# 2.6 비동기 처리

자바스크립트는 싱글 스레드 기반의 프로그래밍 언어다. 이러한 한계와 더불어 유저 인터페이스와 매우 밀접하게 엮인 언어로 시작했다는 특징 때문에, 동기 처리를 이용한 프로그래밍 패턴은 자바스크립트에서 실질적으로 사용불가능하다. 네트워크 요청이 응답을 받을 때까지 싱글 스레드를 점유하고, 그 동안은 브라우저가 유저의 마우스, 키보드에 전혀 반응하지 않는다고 생각해보라!

이런 제약 때문에 비싼 작업을 비동기로 처리하는 것이 자바스크립트에선 매우 흔한 패턴이다. 수 년 전까지만 해도 자바스크립트 코드에서는 그런 비동기 작업이 끝난 후, 그 결과에 따라 추가적인 처리를 하기 위해 비동기 서브루틴에게 콜백 함수를 넘기는 패턴이 자주 쓰였다. 다음 코드 예시는 네트워크에서 문서를 받아온 뒤, 해당 문서를 작성한 유저의 다른 글들을 가져오는 가상의 코드 예시다.

```javascript
fetchDocument(url, function(err, document) {
  if (err) {
    console.log(err);
  } else {
    fetchAuthor(document, function(err, author) {
      if (err) {
        console.log(err);
      } else {
        fetchPostsFromAuthor(author.id, function(err, posts) {
          if (err) {
            console.log(err);
          } else {
            /* do something with posts */
          }
        })
      }
    })
  }
})
```

위 코드에서 볼 수 있듯이 콜백을 사용해 비동기 작업을 처리하면 비동기로 처리하는 작업의 단계가 깊어짐에 따라 들여쓰기 또한 급격히 깊어진다. 이런 코드는 미관상으로도 좋지 않을 뿐더러, 프로그래머가 비동기로 일어나는 일련의 작업 흐름을 따라가기 매우 힘들게 한다. 이런 불편을 해소하고자, 최신 ECMAScript에는 이러한 콜백을 이용한 접근보다 개선된 비동기 처리 패턴이 추가되었다.
