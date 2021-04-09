---
layout: post
categories: posts
title: "[Github Blog] MathJax로 수식 적용하기"
comments: true
use_math: true
---

이 문서에서는 Github Blog에서 수식을 적용하는 방식에 대하여 설명한다. 특히 hydejack 테마 블로그에 수식을 적용하는 방식에 대하여 설명한다. 다른 테마의 블로그일 경우 적용해야하는 요소는 비슷하지만 구체적인 config 파라미터는 다를 수 있다. 이러한 차이가 있음에도 참고할 수 있도록 필자가 참고했던 페이지를 포스트의 마지막에 reference로 추가하였다. 또한 이 문서에 포함되어 있는 내용은 작성일(2021-03-31) 기준 내용이므로 참고하길 바란다.


## [ 필요한 작업 요약 ]
1. \_config.yml 파일 수정 : 수식을 사용하기 위해 Kramdown 렌더러 환경설정을 하기 위함
2. \_includes 디렉토리에 mathjax_support.html 파일 생성 : MathJax를 사용하기 위한 스크립트 작성
3. \_includes 디렉토리의 my-head.html 파일 수정 : 포스트의 헤더파일에 MathJax를 사용하기 위한 스크립트를 적용하기 위함
4. 포스트 작성시 YAML front-matter 설정 : 선택적으로 수식 사용을 하기 위함

위의 내용은 특히 Hydejack 테마 블로그에 맞춰서 작성된 것이고, 블로그의 테마별로 구체적인 설정 방식은 달라질 수 있지만 수식을 적용하기 위한 큰 줄기는 동일할 것이다 [2, 3].


## [ Hydejack 테마 블로그에 MathJax로 수식 적용하는 방법 ]

### 1. \_config.yml 파일 수정
Jekyll 블로그에서는 markdown을 렌더링(rendering)하기 위해서 몇가지 렌더러를 지원한다. 예를 들면  Kramdown, CommonMark, Redcarpet등이 있다 [1]. 각 렌더러마다 지원하는 기능이 조금씩 다른데, 수식 표현을 하기 위해사는 보통 Kramdown을 사용한다. 따라서 수식을 사용하려면 블로그에서 Kramdown 렌더러를 사용할 수 있게 환경설정 해주어야한다. 특히 hydejack 테마 블로그에서는 \_config.yml 파일에서 kramdown에 관련된 설정을 아래와 같이 math\_engine과 math\_engine_opts을 설정해주어야 한다.

참고로 hydejack 테마가 아닌 다른 테마의 경우 \_config.yml 파일에서 Kramdown 렌더러로 환경설정 하는 방식이 다를 수 있다.

```yml 
kramdown:
    footnote_backlink:   '&#x21a9;&#xfe0e;'
    math_engine:         mathjax
    math_engine_opts:
        preview:           true
        preview_as_code:   true
```

### 2. \_includes 디렉토리에 mathjax_support.html 파일 생성
포스트 내에서 수식을 표현하는 기능을 선택적으로 사용할 수 있도록 하기 위해 \_include 디렉토리 내에 mathjax를 사용할 수 있도록 하는 스크립트 코드가 담긴 mathjax_support.html 파일을 생성한다.

```html
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        TeX: {
            equationNumbers: {
                autoNumber: "AMS"
            }
        },
        tex2jax: {
            inlineMath: [ ['$', '$'] ],
            displayMath: [ ['$$', '$$'] ],
            processEscapes: true,
        }
    });
    MathJax.Hub.Register.MessageHook("Math Processing Error",function (message){
        alert("Math Processing Error: "+message[1]);
    });
    MathJax.Hub.Register.MessageHook("TeX Jax - parse error",function (message){
        alert("Math Processing Error: "+message[1]);
    });
</script>
<script type="text/javascript" async
      src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```

### 3. \_includes 디렉토리의 my-head.html 파일 수정
수식을 표현하는 기능을 사용하고자 했을때, 모든 포스트 문서의 헤더에 포함될 수 있도록 헤더로 포함되는 html 파일 코드의 맨 아래 부분에 아래의 내용을 삽입한다. 참고로 아래 코드의 내용은 YAML front-matter 설정에서 use\_math 파라미터가 True일 경우 mathjax\_support.html 스크립트를 실행시킨다는 의미이다. post.use\_math가 아니라 page.use\_math인 이유는 post.html layout이 page.html layout을 베이스로 돌아가기 때문으로 보인다.

hydejack 테마의 경우 my-head.html 파일이지만 다른 테마의 경우 다른 이름일 수도 있다.

{% raw %}

```html
{% if page.use_math %}
    {% include mathjax_support.html %}
{% endif %}
```

{% endraw %}

### 4. 포스트 작성시 YAML front-matter 설정
포스트를 작성할 때 사용하는 YAML front-matter의 종류는 테마 종류별로 또는 사용자가 정의한 방식 등에 따라 다를 수 있다. 하지만 여기서 중요한 포인트는 위의 3번에서 mathjax를 사용하기 위해 미리 정의한 파라미터인 use\_math를 true로 설정했다는 것이다.

```
---
layout: post
categories: posts
title: "[Github Blog] MathJax로 수식 적용하기"
comments: true
use_math: true
---
```

## [ 사용 예시 ]

### 1. Inline

- <b>Example</b>:

	Lorem ipsum $f(x) = x^2$.
	
- <b>Markdown</b>:

	```latex
	Lorem ipsum $f(x) = x^2$.
	```

### 2. Block

- <b>Example</b>:

	$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}$$
	
	
- <b>Markdown</b>:

	```latex
   $$x = {-b \pm \sqrt{b^2-4ac} \over 2a}$$
```


## Reference
- [1] [Jekyll 마크다운 옵션](https://jekyllrb-ko.github.io/docs/configuration/markdown)
- [2] [Hydejack 테마 블로그에 MathJax로 수식 적용하기 참고](https://airvw.github.io/github/2020-12-14-blog-mathjax/)
- [3] [다른 테마 블로그에 MathJax로 수식 적용하기 참고](https://mkkim85.github.io/blog-apply-mathjax-to-jekyll-and-github-pages/)


<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-MML-AM_CHTML' async></script>


<style>
.MJXc-display {
    display: block
}
</style>