# blackhole-pi
Reverse proxy with pihole ansible playbooks

Manual steps:
`sudo apt update`
`sudo apt upgrade -y`

### To Set fixed IP (update IP as appropriate):
`sudo nmcli connection modify "preconfigured" ipv4.addresses 192.168.2.253/24`

`sudo nmcli connection modify "preconfigured" ipv4.gateway 192.168.2.1`

`sudo nmcli connection modify "preconfigured" ipv4.dns 192.168.2.1`

`sudo nmcli connection modify "preconfigured" ipv4.method manual`

# Install pihole
`curl -sSL https://install.pi-hole.net | bash`

update password:
`sudo pihole setpassword`

update gravity list:
`sudo pihole -g`


update block lists under Domain, lookup here: https://firebog.net/

https://raw.githubusercontent.com/PolishFiltersTeam/KADhosts/master/KADhosts.txt
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Spam/hosts
https://v.firebog.net/hosts/static/w3kbl.txt

https://adaway.org/hosts.txt
https://v.firebog.net/hosts/AdguardDNS.txt
https://v.firebog.net/hosts/Admiral.txt
https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt
https://v.firebog.net/hosts/Easylist.txt
https://pgl.yoyo.org/adservers/serverlist.php?hostformat=hosts&showintro=0&mimetype=plaintext
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/UncheckyAds/hosts
https://raw.githubusercontent.com/bigdargon/hostsVN/master/hosts

https://v.firebog.net/hosts/Easyprivacy.txt
https://v.firebog.net/hosts/Prigent-Ads.txt
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts
https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt
https://hostfiles.frogeye.fr/firstparty-trackers-hosts.txt

https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareHosts.txt
https://v.firebog.net/hosts/Prigent-Crypto.txt
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Risk/hosts
https://bitbucket.org/ethanr/dns-blacklists/raw/8575c9f96e5b4a1308f2f12394abd86d0927a4a0/bad_lists/Mandiant_APT1_Report_Appendix_D.txt
https://phishing.army/download/phishing_army_blocklist_extended.txt
https://gitlab.com/quidsup/notrack-blocklists/raw/master/notrack-malware.txt
https://v.firebog.net/hosts/RPiList-Malware.txt
https://raw.githubusercontent.com/Spam404/lists/master/main-blacklist.txt
https://raw.githubusercontent.com/AssoEchap/stalkerware-indicators/master/generated/hosts
https://urlhaus.abuse.ch/downloads/hostfile/
https://lists.cyberhost.uk/malware.txt


# Caddy local only mode:
`sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl`
`curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg`
`curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list`
`sudo apt update`
`sudo apt install caddy`


change default pihole port:
/etc/pihole/pihole.toml

add to /etc/caddy/Caddyfile:

# Jellyfin reverse proxy (local network only)
http://jellyfin.home {
    reverse_proxy 192.168.2.179:8096
}

# Pi-hole reverse proxy (local network only)
http://pihole.home {
    reverse_proxy 192.168.2.253:8080
}

# AudioBookShelf reverse proxy (local network only)
http://abs.home {
    reverse_proxy 192.168.2.179:13378
}

#  Prowlarr reverse proxy (local network only)
http://prowlarr.home {
    reverse_proxy 192.168.2.179:9696
}

#  Radarr reverse proxy (local network only)
http://radarr.home {
    reverse_proxy 192.168.2.179:7878
}

#  Sonarr reverse proxy (local network only)
http://sonarr.home {
    reverse_proxy 192.168.2.179:8989
}

#  qbittorent reverse proxy (local network only)
http://qbittorent.home {
    reverse_proxy 192.168.2.179:9080
}

#  Bazarr reverse proxy (local network only)
http://bazarr.home {
    reverse_proxy 192.168.2.179:6767
}

#  Grafana reverse proxy (local network only)
http://grafana.home {
    reverse_proxy 192.168.2.179:3001
}

#  Index page reverse proxy (local network only)
http://index.home {
    reverse_proxy 192.168.2.179:1111
}

#  Komga reverse proxy (local network only)
http://komga.home {
    reverse_proxy 192.168.2.179:25600
}

#  Nextcloud reverse proxy (local network only)
http://nextcloud.home {
    reverse_proxy 192.168.2.179:8000
}

#  NTFY reverse proxy (local network only)
http://ntfy.home {
    reverse_proxy 192.168.2.179:8090
}

#  Tandoor reverse proxy (local network only)
http://tandoor.home {
    reverse_proxy 192.168.2.179:8081
}

#  immich reverse proxy (local network only)
http://immich.home {
    reverse_proxy 192.168.2.179:2283
}

on pihole admin gui:
Go to the Local DNS Records section: Local DNS > DNS Records.

Add entries like:

jellyfin.home → 192.168.2.253 (your first Raspberry Pi's IP which runs the reverse proxy)
pihole.home → 192.168.2.253
abs.home → 192.168.2.253
prowlarr.home → 192.168.2.253
radar.home → 192.168.2.253
sonarr.home → 192.168.2.253
qbittorent.home → 192.168.2.253
bazarr.home → 192.168.2.253
grafana.home → 192.168.2.253
index.home → 192.168.2.253
komga.home → 192.168.2.253
nextcloud.home → 192.168.2.253
ntfy.home → 192.168.2.253
tandoor.home → 192.168.2.253
immich.home → 192.168.2.253






restart pihole: `sudo systemctl restart pihole-FTL`





## Ansible:
device must be named: pi-dns-proxy-filter
to run playbook:
`ansible-playbook main.yaml`












































































# set up shortcut url to disable for 5 mins:


http://192.168.2.253/admin/api.php?disable=300&auth=PWHASH

on router under DCHP set DNS to pi ip

#Set up unbound:

`sudo apt install unbound -y`

`sudo nano -w /etc/unbound/unbound.conf.d/pi-hole.conf`
past this:

server:
# If no logfile is specified, syslog is used
# logfile: "/var/log/unbound/unbound.log"
verbosity: 0

interface: 127.0.0.1
port: 5335
do-ip4: yes
do-udp: yes
do-tcp: yes

# May be set to yes if you have IPv6 connectivity
do-ip6: no

# You want to leave this to no unless you have *native* IPv6. With 6to4 and
# Terredo tunnels your web browser should favor IPv4 for the same reasons
prefer-ip6: no

# Use this only when you downloaded the list of primary root servers!
# If you use the default dns-root-data package, unbound will find it automatically
#root-hints: "/var/lib/unbound/root.hints"

# Trust glue only if it is within the server's authority
harden-glue: yes

# Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
harden-dnssec-stripped: yes

# Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes
# see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
use-caps-for-id: no

# Reduce EDNS reassembly buffer size.
# IP fragmentation is unreliable on the Internet today, and can cause
# transmission failures when large DNS messages are sent via UDP. Even
# when fragmentation does work, it may not be secure; it is theoretically
# possible to spoof parts of a fragmented DNS message, without easy
# detection at the receiving end. Recently, there was an excellent study
# >>> Defragmenting DNS - Determining the optimal maximum UDP response size for DNS <<<
# by Axel Koolhaas, and Tjeerd Slokker (https://indico.dns-oarc.net/event/36/contributions/776/)
# in collaboration with NLnet Labs explored DNS using real world data from the
# the RIPE Atlas probes and the researchers suggested different values for
# IPv4 and IPv6 and in different scenarios. They advise that servers should
# be configured to limit DNS messages sent over UDP to a size that will not
# trigger fragmentation on typical network links. DNS servers can switch
# from UDP to TCP when a DNS response is too big to fit in this limited
# buffer size. This value has also been suggested in DNS Flag Day 2020.
edns-buffer-size: 1232

# Perform prefetching of close to expired message cache entries
# This only applies to domains that have been frequently queried
prefetch: yes

# One thread should be sufficient, can be increased on beefy machines. In reality for most users running on small networks or on a single machine, it should be unnecessary to seek performance enhancement by increasing num-threads above 1.
num-threads: 1

# Ensure kernel buffer is large enough to not lose messages in traffic spikes
so-rcvbuf: 1m

# Ensure privacy of local IP ranges
private-address: 192.168.0.0/16
private-address: 169.254.0.0/16
private-address: 172.16.0.0/12
private-address: 10.0.0.0/8
private-address: fd00::/8
private-address: fe80::/10




`sudo service unbound restart`

`sudo service unbound status`

test:
`dig crosstalksolutions.com @127.0.0.1 -p 5335`