---
layout: default
title: 한국인을 위한 포스팅
description: 이곳은 국어로 적힌 포스트를 모아둔 곳 입니다.. 
permalink: /kor/
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
    border-collapse: collapse;
    table-layout: fixed; 컬럼 길이 고정
    width: auto; /* 테이블 너비를 내용에 맞게 자동 조정 */
}

th, td {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
    white-space: nowrap; /* 텍스트 줄바꿈 방지 */
    width: 400px; /* 컬럼 길이 고정 */

}

th {
    background-color: #f2f2f2;
}

th:nth-child(1), td:nth-child(1) {
    width: 150px; /* 첫 번째 컬럼의 너비 */
}

th:nth-child(2), td:nth-child(2) {
    width: 750px; /* 두 번째 컬럼의 너비 */
}

th:nth-child(3), td:nth-child(3) {
    width: 300px; /* 세 번째 컬럼의 너비 */
}

.styled-table {
    width: 100%;
    border-collapse: collapse;
    background-color: #ffffff;
}

.styled-table tr {
    border-bottom: 1px solid #dddddd;
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
}

.access-type {
    color: red;
}

.date {
    color: #666666;
}

.content {
    padding: 20px;
    vertical-align: top;
}

.title {
    font-size: 18px;
    font-weight: bold;
    margin: 0 0 10px;
}

.description {
    margin: 0 0 10px;
    color: #555555;
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

.thumbnail img {
    width: 260px;
    height: 150px;
    display: block;
    object-fit: cover;
}

</style>

<div class="table-container">
        <table class="styled-table">
            <tr>
                <td class="meta-data">
                    <div class="article-type">Article</div>
                    <div class="access-type">Open Access</div>
                    <div class="date">10 Jul 2024</div>
                </td>
                <td class="content">
                    <h2 class="title">The quantum transition of the two-dimensional Ising spin glass</h2>
                    <p class="description">We find that, in the quantum transition of Ising spin glass, the closing of the gap at the critical point can remain algebraic by restricting the symmetry of possible excitations, which is crucial for quantum annealing.</p>
                    <p class="authors">Massimo Bernaschi, Isidoro González-Adalid Pemartín ... Giorgio Parisi</p>
                </td>
                <td class="thumbnail">
                    <img src="/assets/char.png" alt="Article Image">
                </td>
            </tr>
            <tr>
                <td class="meta-data">
                    <div class="article-type">Article</div>
                    <div class="access-type">Open Access</div>
                    <div class="date">10 Jul 2024</div>
                </td>
                <td class="content">
                    <h2 class="title">A liver immune rheostat regulates CD8 T cell immunity in chronic HBV infection</h2>
                    <p class="description">A liver-intrinsic mechanism is presented that suppresses effective anti-hepatitis virus B responses in mice and humans by rendering virus-specific CD8 T cells refractory to activation causing loss of effector functions.</p>
                    <p class="authors">Miriam Bosch, Nina Kallin ... Percy A. Knolle</p>
                </td>
                <td class="thumbnail">
                    <img src="/assets/char.png" alt="Article Image">
                </td>
            </tr>
        </table>
    </div>

