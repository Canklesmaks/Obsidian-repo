 # 1. convert IP and subnet mask to decimal
* eg 165.245.12.88/24  |   subnet mask = 255.255.255.0 
# 2. determine network/subnet address
*  Network address: if mask is 255, bring down address. if mask is 0 use 0.
	* so, network address for 165.245.12.88/24  = 165.245.12.0
# 3. determine broadcast address
* Broadcast address: if mask is 255 bring the address, if mask is 0 use 255.
	* so, broadcast address for 165.245.12.88/24 = 165.245.12.255
# 4. calculate first and last usable IP
* First IP in range of addresses is network address + 1 (165.245.12.0)
* Last IP in range of addresses is broadcast address - 1 (165.245.12.254)


![[Pasted image 20230622155939.png]]
![[Pasted image 20230621144030.png]]


# 1. convert IP and subnet mask to decimal
* eg 165.245.12.88/26  |   subnet mask = 255.255.255.192
# 2. determine network/subnet address
*  Network address: if mask is 255, bring down address. if mask is 0 use 0. 
* FOR ANY OTHER NUMBER USE THE CHART.
	* so, network address for 165.245.12.88/26  = 165.245.12.64
# 3. determine broadcast address
*    Broadcast address: if mask is 255 bring the address, if mask is 0 use 255.
* FOR ANY OTHER NUMBER USE THE CHART.
	* so, broadcast address for 165.245.12.88/26 = 165.245.12.127
# 4. calculate first and last usable IP
* First IP in range of addresses is network address + 1 (165.245.1.64)
* Last IP in range of addresses is broadcast address - 1 (165.245.12.126)

