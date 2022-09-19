--소셜 로그인--

@GetMapping("/kakao")
public UserDTO kakaoCallback(@Param("code") String code) throws IOException {

    String[] access_Token = userService.getKaKaoAccessToken(code);
    String access_found_in_token = access_Token[0];

    //배열로 받은 토큰들의 accsess_token만 createKaKaoUser 메서드로 전달
    UserDTO kakaoUser = userService.createKakaoUser(access_found_in_token);

    return kakaoUser;

}

public String[] getKaKaoAccessToken(String code) {
String access_Token = "";
String refresh_Token = "";
String reqURL = "https://kauth.kakao.com/oauth/token";

    String result = null;
    String id_token = null;
    try {
      URL url = new URL(reqURL);
      HttpURLConnection conn = (HttpURLConnection) url.openConnection();

      //POST 요청을 위해 기본값이 false인 setDoOutput을 true로
      conn.setRequestMethod("POST");
      conn.setDoOutput(true);

      //POST 요청에 필요로 요구하는 파라미터 스트림을 통해 전송
      BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(conn.getOutputStream()));
      StringBuilder sb = new StringBuilder();
      sb.append("grant_type=authorization_code");
      sb.append("&client_id=4af7c95054f7e1d31cff647965678936"); // TODO REST_API_KEY 입력
      sb.append("&redirect_uri=http://localhost:3000/kakaologin"); // TODO 인가코드 받은 redirect_uri 입력
      System.out.println("code = " + code);
      sb.append("&code=" + code);
      bw.write(sb.toString());
      bw.flush();

      //결과 코드가 200이라면 성공
      int responseCode = conn.getResponseCode();
      System.out.println("responseCode : " + responseCode);
      //요청을 통해 얻은 JSON타입의 Response 메세지 읽어오기
      BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
      String line = "";
      result = "";

      while ((line = br.readLine()) != null) {
        result += line;
      }
      // bearer 토큰 값만 추출(log에 찍히는 값의 이름은 id_Token)
      System.out.println("response body : " + result);
      String[] temp = result.split(",");
      id_token = temp[3].substring(11);
      System.out.println("idToken = " + id_token);

// Gson 라이브러리에 포함된 클래스로 JSON파싱 객체 생성
JsonParser parser = new JsonParser();
JsonElement element = parser.parse(result);

      access_Token = element.getAsJsonObject().get("access_token").getAsString();
      refresh_Token = element.getAsJsonObject().get("refresh_token").getAsString();

      System.out.println("access_token : " + access_Token);
      System.out.println("refresh_token : " + refresh_Token);

      br.close();
      bw.close();

    } catch (Exception e) {
      e.printStackTrace();
    }
    String[] arrTokens = new String[3];
    arrTokens[0] = access_Token;
    arrTokens[1] = refresh_Token;
    arrTokens[2] = id_token;

    // token 값들 배열로 리턴(프론트에서 쓰일지도 모르기 때문)
    return arrTokens;

}
