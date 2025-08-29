# Setup docker-mailserver

## Minimal DNS Setup

- `A` for `mx.example.com` contains `11.22.33.44`
- `MX` for `example.com` contains `mx.example.com`
- `PTR` for `example.com` contains `mx.example.com`

If you setup everything, it should roughly look like this:

```bash
$ dig @1.1.1.1 +short MX example.com
mx.example.com
$ dig @1.1.1.1 +short A mx.example.com
11.22.33.44
$ dig @1.1.1.1 +short -x 11.22.33.44
mx.example.com
```

## Configuration

```bash
mkdir ~/dockermail
cd ~/dockermail

DMS_GITHUB_URL="https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/master"
curl -O "${DMS_GITHUB_URL}/compose.yaml"
curl -O "${DMS_GITHUB_URL}/mailserver.env"
```

```bash
sudo ufw allow 25
sudo ufw allow 143
sudo ufw allow 465
sudo ufw allow 587
sudo ufw allow 993
```


## References

GitHub https://github.com/docker-mailserver/docker-mailserver
Documentation https://docker-mailserver.github.io/docker-mailserver/latest/
