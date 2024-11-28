In generalized forwarding, a match-plus-action table generalizes notion of destination-based forwarding
- Instead of layer 3 routers or layer 2 switches, we generally refer to them as **packet switches**
- In practice, these functionalities are implemented by a remote controller

## OpenFlow
- Each entry in match-plus-action **flow table** includes:
	- **Set of header field values** to which incoming packets will be matched 