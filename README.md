# binardat-switch-exporter
An attempt to scrape port statistics from Binardat managed switches, based on cheap-switch-exporter

http://binardat.com/download/2.5g-switch-manual-binardat.pdf

These switches are very similar to Sodala and Horaco switches that are supported by this project: https://github.com/pvelati/cheap-switch-exporter

I purchased the 4 port 2.5G web managed switch with 2x10G SFP+ Slot ( https://www.amazon.com/dp/B0CWNZWFTS?ref=ppx_yo2ov_dt_b_fed_asin_title&th=1 )

## Purpose
The web interface seems to be an update of the web interface and uses a different login process and thus the original project will not work. Instead of using FORM data, a time limited cookie is used. Incredibly, you don't appear to need to successfully authenticate in order to receive this cookie!

## Proof of Concept
### Get Cookie
Using Curl, send a POST request to the login.cgi URL of the web interface and get back the cookie.  Both of these work:

```
curl -X POST http://192.168.2.1/login.cgi -d "data=username=admin&password=admin" -c /tmp/cookies.txt
curl -X POST http://192.168.2.1/login.cgi -c /tmp/cookies.txt
```
In fact, while testing this for the README, I discovered you don't even need it to be a POST, a simple GET request will "get" the needed cookie!

### Get Stats
After you have the cookie, you can then scrape the stats page:
```
curl "http://192.168.2.1/port.cgi?page=stats" --cookie /tmp/cookies.txt
```

## Lack of Knowledge
Without the help of Google, I'd probably no nothing about the Go programming language, but I've been hacking up the original program by pvelati to try and make it work. So far, I can't seem to get the cookie to be accepted when requesting the stats.

## Acknowledgements
All credit should go to [Paolo Velati](https://github.com/pvelati)

