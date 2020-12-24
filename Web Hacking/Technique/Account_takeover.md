## Ways of taking over account

#### Password reset link

- Try include attacker's email in request (e.g. user=victim@email.com&user=attacker@email.com) and see if it sends password reset link to attacker. 

- Try make 2 requests for password reset with victim e-mail and attacker's e-mail with same session cookies. In some cases, password link would be sent to both e-mails.

- To submit registration, try intercept and add "X-Forwarded-For", "X-Forwarded-Host" and add evil.com. Check if the confirmation link would be showing evil.com

- Check if token in Password reset link is weakly encrypted. Check "timestamp", "base64" or reuse password token in other accounts.

#### Change password
- Try change attacker's password but adding victim's e-mail in request. Check if it would change victim pasword too.

- Check if changing account details (including password) include csrf token in its POST request. (Please refer csrf attack for more details)

#### OTP
- Check if OTP can be bruteforced, reused in other accounts  or shown in initial request.

#### Login Bruteforce
- Check if adding %00 space, %0d , %2e , %09 , %20 after username can bypass rate limit.