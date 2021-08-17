## ifconfig 란?

* `en0` stands for "EtherNet"

```bash
# wifi 내려보고 상태값 비교해보기
$ ifconfig en0 down

$ ifconfig en0 up
```



### ifconfig 분석해보기 

```bash
$ ifconfig en0

en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=400<CHANNEL_IO>
	# mac address 
	ether 3c:22:fb:a3:02:28 
	# IPv6 
	inet6 fe80::1c76:9b94:1e4f:4b88%en0 prefixlen 64 secured scopeid 0x6 
	# private ip address (nobody outside can't see)
	inet 192.168.219.107 netmask 0xffffff00 
	# 다른사람이 볼 수 있는 내 컴퓨터 ip 
	broadcast 192.168.219.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
```



----

### 참조

* https://www.youtube.com/watch?v=5Tke_JD7-jk