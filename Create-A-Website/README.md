# No Need CSR when creating a website with SquareSpace
Squarespace automatically generates and renews an SSL certificate for your website for free, ensuring your visitors have a secure and encrypted connection. This mean you won't have to sign a CSR and send it to a CA manually. If you wish to, read the main page `REAME.md` in `SecOps` folder.
- Notice: Althought it said SSL (an older and less secure version of TLS), it actually use TLS protocol instead of SSL.

# Sub-domain creating
- Go to `https://account.squarespace.com/`, click on `Domains` > `DNS` > into `Google records` > click `ADD RECORD` > choose your subdomain's name in the `HOST` section > choose `TYPE` A > choose your domain's IP address (you can look it at your `Squarespace Defaults` in the row that has the `HOST` `@` or use `ping` command in linux to your domain name to get the IP address)
