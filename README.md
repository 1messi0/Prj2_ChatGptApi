# Prj2_ChatGptApi
챗지피티 연동

OpenApi ChatGPT 연동 Api 사용

## 추가내용 ##
엔터 키 입력 시 결과 전송 기능 추가

## 의문사항 ##
챗지피티 api는 하루가 지나면 응답을 안하는 듯? = 확인중

== 코드내용 ==
<body>
  <h1>챗지피티 연동</h1>
  <div>질문 하단 입력</div>

  <input type="text" id="keywords" name="keywords" required />
  <button onclick="chatGPT()">입력</button>
  <div id="result"></div>

  <div id="loading">
    <img src="https://studentrights.sen.go.kr/images/common/loading.gif">
  </div>
  <script>
    $(document).ready(function () {
      $('#loading').hide();

      // 엔터 키 이벤트 감지
      $('#keywords').keydown(function (e) {
        if (e.keyCode == 13) {
          chatGPT(); // 엔터 키가 눌리면 chatGPT 함수 호출
        }
      });
    });

    function chatGPT() {
      const api_key = ""  // <- API KEY 입력
      const keywords = document.getElementById('keywords').value
      $('#loading').show();

      const messages = [
        { role: 'system', content: 'You are a helpful assistant.' },
        { role: 'user', content: keywords + '에 대하여 최대한 도움이 되는 답변을 해줘.' },
      ]

      const data = {
        model: 'gpt-3.5-turbo',
        temperature: 0.5,
        n: 1,
        messages: messages,
      }

      $.ajax({
        url: "https://api.openai.com/v1/chat/completions",
        method: 'POST',
        headers: {
          Authorization: "Bearer " + api_key,
          'Content-Type': 'application/json',
        },
        data: JSON.stringify(data),
      }).then(function (response) {
        $('#loading').hide();
        console.log(response)
        let result = document.getElementById('result')
        let pre = document.createElement('pre')

        pre.innerHTML = "\n\n" + response.choices[0].message.content
        result.appendChild(pre)

        document.getElementById('keywords').value = ''
      });
    }
  </script>
</body>
