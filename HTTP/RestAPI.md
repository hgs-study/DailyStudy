## Server Response
+ ResponseEntity
``` 
	@PostMapping(value="/mypage/user/referralChk/chk")
	public ResponseEntity<MessageDTO> referralChk(@RequestParam String referral_id) throws URISyntaxException{
		
		MessageDTO message = MessageDTO.builder()
								.message("정상적으로 등록되었습니다.")
								.build();
		
		URI uri = new URI("/main");
		HttpHeaders headers = new HttpHeaders();
		headers.setLocation(uri);
		
		return ResponseEntity.ok().headers(headers).body(message);
    //return new ResponseEntity<String>(headers,HttpStatus.OK);
	}
```

+ return ResponseEntity.ok().headers(headers).body(message);
  - 상태 코드를 200 ok라고 전달
  - 헤더에 Location 정보를 보냄, 리디렉션
  - 바디에 message를 함께 정상적으로 등록되었다고 보냄
  
+ return new ResponseEntity<String>(headers,HttpStatus.OK);
  - 헤더 정보과 상태코드를 보낸다(상태코드 100~500)
