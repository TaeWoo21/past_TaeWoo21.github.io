---
layout: post
categories: posts
title: GitHub - 협업할 때 사용하는 Git 브랜치 종류 및 사용법
comments: true
---

이 문서에서는 AWS(Amazon Web Services)에서 S3 기반으로 HBase를 구축하는 방법부터 그 위에 OpenTSDB를 설치하는 방법까지 설명한다.

이 문서는 기본적으로 [[GitHub] Git 브랜치의 종류 및 사용법 (5가지)](https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html)를 참고하여 작성하였다. 또한 이 문서에 포함되어 있는 내용은 작성일(2021-02-16) 기준 내용이므로 참고하길 바란다.


## [ Feature branch ] 

새로운 기능 개발 및 버그 수정을 위한 브랜치이다. feature 브랜치는 develop 브랜치에서 분기하여 개발한다.

feat/[기능요약] 형식으로 사용하는 것을 추천

### bash commands 예시

```
# (1)
# feature 브랜치(feat/fast-preprocess)를 develop 브랜치에서 분기한다.
# [브랜치명]으로 새로운 브랜치를 만들면서 체크아웃 한다(git checkout -b [브랜치명])
$ git checkout -b feat/fast-preprocess develop

(... 새로운 작업 수행 ...)

# (2)
# 새로운 작업이 모두 끝나면 develop 브랜치로 이동
$ git checkout develop

# (3)
# develp 브랜치에 feat/fast-preprocess 브랜치 내용을 병합(merge)
# --no-ff: feature 브랜치에 존재하는 커밋 이력을 모두 합쳐서 하나의 새로운 커밋 객체로 만들어 develop 브랜치로 병합하는 옵션
$ git merge --no-ff feat/fast-preprocess

# (4)
# 병합된 feature 브랜치를 삭제
$ git branch -d feat/fast-preprocess

# (5)
# develop 브랜치를 원격 중앙 저장소에 올리기
$ git push origin develop

# (6)
# feature 브랜치 원격 중앙 저장소에서도 삭제
$ git push origin :feat/fast-preprocess
```