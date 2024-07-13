---
layout: default
description: Main Papers for Interpretability Research
title: Papers
permalink: /main_papers/
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

.link {
    color: #F95893;
    font-size: 14px;
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
                    <div class="article-type">Workshop</div>
                    <div class="access-type">KCC XAI</div>
                    <div class="date">28 June 2024</div> <br>
                    <a class="link" href="https://1drv.ms/b/s!Asr4ZEBiKgSu31uRiEXSMlhorkei?e=iuAz1Z" > [pdf] </a> <br>
                    <a class="link" href="https://room1805.github.io/" > room 1805 🔗 </a> 
                </td>
                <td class="content">
                    <h2 class="content-title">Representation Interpretation of Refusal Mechanism In Large Language Models</h2>
                    <p class="description">We adapt mechanistic interpretability on the refusal mechanism by proposing Prompt and Location-based Refusal Analysis (PLoRA), which compares refusal feature vectors generated from the specific prompts and at the locations of tokens.</p>
                    <p class="authors">Bumjin Park, Yeonjea Kim, Jinsil Lee, Youngju Joung and Jaesik Choi</p>
                </td>
                <td class="thumbnail">
                    <img src="https://room1805.github.io/assets/img/room1806/2.png" alt="Article Image">
                </td>
            </tr>
            <tr>
                <td class="meta-data">
                    <div class="article-type">Conference</div>
                    <div class="access-type">IJCAI</div>
                    <div class="date">09 Aug 2024</div> <br>
                    <a class="link" href="/main_papers/2024_guidace_loss_for_documents" > [project] </a> <br>
                    <a class="link" href="https://arxiv.org/abs/2406.15996" > [arxiv] </a>
                </td>
                <td class="content">
                    <h2 class="content-title">Memorizing Documents with Guidance in Large Language Models </h2>
                    <p class="description">We propose document-wise memories, which makes document-wise locations for neural memories in training. The proposed architecture maps document representation to memory entries and filters memory selections in the forward process of LLMs.</p>
                    <p class="authors">Bumjin Park and Jaesik Choi</p>
                </td>
                <td class="thumbnail">
                    <img src="/assets/main_papers/2024_guidance_loss_fig_update2.png" alt="Article Image">
                </td>
            </tr>
            <tr>
                <td class="meta-data">
                    <div class="article-type">Conference</div>
                    <div class="access-type">ICPRAI</div>
                    <div class="date">12 June 2024</div>
                    <br>
                    <a class="link" href="/main_papers/2024_icprai_source_identification" > [project] </a>
                </td>
                <td class="content">
                    <h2 class="content-title">Identifying the Source of Generation  for Large Language Models</h2>
                    <p class="description">This work introduces token-level source identification in the decoding step, which maps the token representation to the reference document. We propose a bi-gram source identifier which has two successive token representations as input.</p>
                    <p class="authors">Bumjin Park and Jaesik Choi</p>
                </td>
                <td class="thumbnail">
                    <img src="/assets/main_papers/2024_icprai_source_identification.png" alt="Article Image">
                </td>
            </tr>
        </table>
</div>
