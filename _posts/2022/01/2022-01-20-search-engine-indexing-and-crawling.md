---
layout: post
title: Search engine indexing and crawling
category: web
created_date: 2022-01-20
modified_date: 2022-01-23
author: duchangkim
tags: [web, search]
search_keyword: [검색엔진, 검색, 구글, 크롤링, 크롤러, 인덱싱]
---

***
회사 내부 직원들이 사용할 관리센터 페이지를 개발하던 도중 이 페이지가 과연 검색엔진에 노출되어도 괜찮을까? 하는 생각이 들었습니다. 관리 페이지 이기도 하고, 구글이나 네이버에 검색했을 때 나와야 할 필요도 없을 것 같아서 관련 내용들을 찾아봤습니다.

# 구글 검색
온라인에 존재하는 웹사이트는 아마 셀 수 없이 많을 것 입니다. 수백억개 이상 이겠죠..? 그렇다면 검색 엔진에서 검색을 했을 때, 실시간으로 웹사이트를 모두 일일이 방문해서 나온 결과를 사용자에게 보여주는 것 일까요?  

구글 검색은 실제로 웹을 검색하는 것이 아닙니다. 구글의 웹 색인을 검색하는 것이지요. 이 색인이 무엇이며, 구글은 어떤 방법으로 색인을 구성하는지 알아봅시다.

## 크롤링(Crawling)
웹페이지의 데이터들을 수집하기 위해 가장 먼저 하는 일이 '크롤링'입니다. 구글은 스파이더라고 부르는 소프트웨어가 웹페이지 몇 개를 가져오는 것으로 시작해서 그 페이지에서 데이터를 수집하고, 페이지에 연결된 링크를 따라갑니다. 새로 가리킨 페이지에서도 데이터를 수집합니다. 이 과정을 반복하면서 데이터를 수집하게 됩니다.  

<a href="https://lyb1495.tistory.com/17" target="_blank">https://lyb1495.tistory.com/17</a> 👈크롤러에 관한 자세한 설명은 이 글을 참고하세요.

## 인덱싱(Indexing)
크롤러(스파이더)가 찾아 가져온 웹사이트의 정보를 구글 검색 색인에 저장하는 것을 뜻합니다. 가져온 데이터는 구글의 분류법에 따라 분류해 저장합니다. 끝없이 커지고있는 도서관과 같다고 볼 수 있습니다.

## 실제 검색
치타의 달리기 속도가 궁금하다고 가정해 봅시다. 구글 검색창에 '치타 달리기 속도'를 타이핑하고, return을 누릅니다. 그러면 소프트웨어는 각 검색어를 포함하는 모든 페이지를 찾기위해 구글 색인을 검색하게 됩니다. 이 때 가능한 검색결과가 수십만 가지 나옵니다. 그런데 어떻게 구글은 한 페이지에 양질의 검색 결과를 노출하는 것일까요?  
구글에서 정한 200가지가 넘는 질문을 거칩니다. 그리고 그것을 종합하여 페이지의 전반적인 평점을 매기고 이에 따라 검색결과를 내보내게 됩니다.


### ref  
<a href="https://www.google.com/intl/ko/search/howsearchworks/crawling-indexing/" target="_blank">https://www.google.com/intl/ko/search/howsearchworks/crawling-indexing/</a>  
<a href="https://hypemarc.com/seo-crawling-indexing/" target="_blank">https://hypemarc.com/seo-crawling-indexing/</a>