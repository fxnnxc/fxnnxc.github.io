---
layout: default
description: Main Articles for Interpretability Research
title: Main Articles
permalink: /main_articles/
---



<style>

body {
    font-family: Arial, sans-serif;
}

.table-container {
    /* overflow-x: auto; 테이블이 컨테이너 밖으로 나가지 않도록 스크롤 생성 */
    width: 100%; /* 필요한 경우 컨테이너 너비 조절 */
    display: flex; /* 컨테이너를 flex로 설정 */
    justify-content: center; /* 수평 가운데 정렬 */
}

table {
    /* border-collapse: collapse; */
    table-layout: fixed; 컬럼 길이 고정
    width: auto; /* 테이블 너비를 내용에 맞게 자동 조정 */
    border: none; /* 테이블 셀의 테두리를 제거 */
    box-shadow: none;

}

th, td {
    /* border: 1px solid #dddddd; */
    text-align: left;
    padding: 8px;
    white-space: nowrap; /* 텍스트 줄바꿈 방지 */
    border: none; /* 테이블 셀의 테두리를 제거 */
}

tr {
  border-bottom: 1px solid #999999;
  border-top: 1px solid #999999;
}

th {
    background-color: #f2f2f2;
}

th:nth-child(1), td:nth-child(1) {
    width: 250px; /* 첫 번째 컬럼의 너비 */
}

th:nth-child(2), td:nth-child(2) {
    width: 850px; /* 두 번째 컬럼의 너비 */
}


.styled-table {
    width: 100%;
    border-collapse: collapse;
    background-color: #ffffff;
}

.styled-table tr {
    /* border-bottom: 1px solid #dddddd; */
}

.styled-table td {
    word-wrap: break-word;   
    overflow-wrap: break-word;
    vertical-align: top;
    text-overflow: ellipsis;
    white-space: normal;
}


.meta-data {
    width: 120px;
    padding: 20px;
    vertical-align: top;
    font-size: 14px;
    color: #333333;
}

.article-type {
    font-weight: bold;
    color: #009900;
    font-size: 18px;

}

.access-type {
    color: #009900;
    font-size: 17px;

}

.link {
    color: #F95893;
    font-size: 16px;
}


.date {
    color: #666666;
}

.content {
    padding: 20px;
    vertical-align: top;
}

.content-title {
    font-size: 18px;
    font-weight: bold;
    margin: 0 0 10px;
}

.description {
    margin: 0 0 10px;
    color: #555555;
}

a {
  margin: 0 0 0 0;
}

.authors {
    margin: 0;
    color: #999999;
}

.thumbnail {
    width: 150px;
    padding: 20px;
    vertical-align: top;
    text-align: center;
}

</style>



<div class="table-container">
        <table class="styled-table">
            <tr>
                <td class="meta-data">
                    <div class="article-type">Literature Review</div>
                    <div class="access-type">Copyright Issue</div>
                    <div class="date">16 Jul 2024</div> <br>
                    <a class="link" href="/main_articles/240716_induction_heads" > [Post] </a> 
                    <a class="link" href="https://arxiv.org/abs/2404.07129"> [Arxiv] </a> <br>
                </td>
                <td class="content">
                    <a class="link" href="/main_articles/240716_induction_heads" > 
                    <h2 class="content-title">Review on "What needs to go right for an induction head? A mechanistic study of in-context learning circuits and their formation"
                    </h2></a> 
                    <p class="description">The emergence of induction heads in training GPT.</p>
                    <p class="authors">Bumjin Park</p>
                </td>
            </tr>
            <tr>
                <td class="meta-data">
                    <div class="article-type">Literature Review</div>
                    <div class="access-type">Copyright Issue</div>
                    <div class="date">15 Jul 2024</div> <br>
                    <a class="link" href="/main_articles/240715_copyright_memo" > [Post] </a> 
                    <a class="link" href="https://scholar.google.com/scholar_url?url=https://www.researchgate.net/profile/A-Cooper-2/publication/381963290_The_Files_are_in_the_Computer_Copyright_Memorization_and_Generative-AI_Systems/links/66864e060a25e27fbc2422dc/The-Files-are-in-the-Computer-Copyright-Memorization-and-Generative-AI-Systems.pdf&hl=en&sa=X&d=301181645666268126&ei=vXCUZt6pKZGP6rQPjKeIiAs&scisig=AFWwaeZH2oG23U8MG2IXUN-14OUF&oi=scholaralrt&hist=XzIXaxoAAAAJ:9420160871089282663:AFWwaeZtekeyAIg982M8R1D4WSk9&html=&pos=0&folt=cit" > [Scholar] </a> <br>
                </td>
                <td class="content">
                    <a class="link" href="/main_articles/240715_copyright_memo" > 
                    <h2 class="content-title">Review on "The Files are in the Computer: Copyright, Memorization, and Generative-AI Systems" 
                    </h2></a> 
                    <p class="description">Copyright is one of the most important issues in the realm of generative AI. </p>
                    <p class="authors">Bumjin Park</p>
                </td>
            </tr>
            <tr>
                <td class="meta-data">
                    <div class="article-type">Literature Review</div>
                    <div class="access-type">Mechanistic Interpretability</div>
                    <div class="date">14 Jul 2024</div> <br>
                    <a class="link" href="/main_articles/240714_m3" > [Post] </a> 
                    <a class="link" href="https://openreview.net/forum?id=qzsDKwGJyB" > [Arxiv] </a> <br>
                </td>
                <td class="content">
                    <a class="link" href="/main_articles/240714_m3" > 
                    <h2 class="content-title">Review on Explicit Memory for Language Modeling ($\text{Memory}^3$) 
                    </h2></a> 
                    <p class="description">Literature reviews for memory architecture in neural networks </p>
                    <p class="authors">Bumjin Park</p>
                </td>
            </tr>
            <tr>
                <td class="meta-data">
                    <div class="article-type">Literature Review</div>
                    <div class="access-type">Mechanistic Interpretability</div>
                    <div class="date">13 Jul 2024</div> <br>
                    <a class="link" href="/main_articles/240713_mi2" > [Post] </a> 
                    <a class="link" href="https://openreview.net/forum?id=qzsDKwGJyB" > [OpenReview] </a> <br>
                </td>
                <td class="content">
                    <a class="link" href="/main_articles/240713_mi2" > 
                    <h2 class="content-title">Review on dictionary learning for mechanistic interpretability in ICML 2024 
                    </h2></a> 
                    <p class="description">Literature reviews on ICML 2024 mechanistic interpretability workshop oral and spotlight papers.</p>
                    <p class="authors">Bumjin Park</p>
                </td>
            </tr>
            <tr>
                <td class="meta-data">
                    <div class="article-type">Literature Review</div>
                    <div class="access-type">Mechanistic Interpretability</div>
                    <div class="date">13 Jul 2024</div> <br>
                    <a class="link" href="/main_articles/240713_mi1" > [Post] </a> 
                    <a class="link" href="https://openreview.net/forum?id=pJs3ZiKBM5" > [OpenReview] </a> <br>
                </td>
                <td class="content">
                    <a class="link" href="/main_articles/240713_mi1" > 
                    <h2 class="content-title">Review on causality for mechanistic interpretability in ICML 2024 
                    </h2></a> 
                    <p class="description">Literature reviews on ICML 2024 mechanistic interpretability workshop oral and spotlight papers.</p>
                    <p class="authors">Bumjin Park</p>
                </td>
            </tr>
        </table>
</div>