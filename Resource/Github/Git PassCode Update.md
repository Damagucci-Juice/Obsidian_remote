- 키체인 삭제후에 
```bash
git config --global credential.helper osxkeychain
git push 
```

- 비밀번호만 재설정
```bash
git config --global --unset user.password
```

- 이름 이메일 업데이트
```bash
git config --global user.name "Bob"  
git config --global user.email "bob@example.com"
```