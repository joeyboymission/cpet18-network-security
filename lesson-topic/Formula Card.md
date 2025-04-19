


## CLASS A

| Subnet Mask | CIDR | Number of Subnets | Number of Total Usable Address |
| ----------- | ---- | ----------------- | ------------------------------ |
| 255.0.0.0   | /8   | 1                 | 16,777,214                     |
| 255.128.0.0 | /9   | 2                 | 8,388,607 per subnet          |
| 255.192.0.0 | /10  | 4                 | 4,194,303 per subnet          |
| 255.224.0.0 | /11  | 8                 | 2,097,151 per subnet          |
| 255.240.0.0 | /12  | 16                | 1,048,575 per subnet          |
| 255.248.0.0 | /13  | 32                | 524,287 per subnet            |
| 255.252.0.0 | /14  | 64                | 262,143 per subnet            |
| 255.254.0.0 | /15  | 128               | 131,071 per subnet            |
| 255.255.0.0 | /16  | 256               | 65,534 per subnet             |
| 255.255.128.0 | /17 | 512              | 32,767 per subnet             |
| 255.255.192.0 | /18 | 1,024            | 16,383 per subnet             |
| 255.255.224.0 | /19 | 2,048            | 8,191 per subnet              |
| 255.255.240.0 | /20 | 4,096            | 4,095 per subnet              |
| 255.255.248.0 | /21 | 8,192            | 2,047 per subnet              |
| 255.255.252.0 | /22 | 16,384           | 1,023 per subnet              |
| 255.255.254.0 | /23 | 32,768           | 511 per subnet                |
| 255.255.255.0 | /24 | 65,536           | 254 per subnet                |

## CLASS B

| Subnet Mask | CIDR | Number of Subnets | Number of Total Usable Address |
| ----------- | ---- | ----------------- | ------------------------------ |
| 255.255.0.0 | /16  | 1                 | 65,534                         |
| 255.255.128.0 | /17 | 2                | 32,767 per subnet             |
| 255.255.192.0 | /18 | 4                | 16,383 per subnet             |
| 255.255.224.0 | /19 | 8                | 8,191 per subnet              |
| 255.255.240.0 | /20 | 16               | 4,095 per subnet              |
| 255.255.248.0 | /21 | 32               | 2,047 per subnet              |
| 255.255.252.0 | /22 | 64               | 1,023 per subnet              |
| 255.255.254.0 | /23 | 128              | 511 per subnet                |
| 255.255.255.0 | /24 | 256              | 254 per subnet                |
| 255.255.255.128 | /25 | 512            | 126 per subnet                |
| 255.255.255.192 | /26 | 1,024          | 62 per subnet                 |
| 255.255.255.224 | /27 | 2,048          | 30 per subnet                 |
| 255.255.255.240 | /28 | 4,096          | 14 per subnet                 |
| 255.255.255.248 | /29 | 8,192          | 6 per subnet                  |
| 255.255.255.252 | /30 | 16,384         | 2 per subnet                  |

## CLASS C

| Subnet Mask | CIDR | Number of Subnets | Number of Total Usable Address |
| ----------- | ---- | ----------------- | ------------------------------ |
| 255.255.255.0 | /24  | 1               | 254                            |
| 255.255.255.128 | /25 | 2              | 126 per subnet                 |
| 255.255.255.192 | /26 | 4              | 62 per subnet                  |
| 255.255.255.224 | /27 | 8              | 30 per subnet                  |
| 255.255.255.240 | /28 | 16             | 14 per subnet                  |
| 255.255.255.248 | /29 | 32             | 6 per subnet                   |
| 255.255.255.252 | /30 | 64             | 2 per subnet                   |

## CLASS D (Multicast)

| Address Range | CIDR | Purpose | Notes |
| ------------- | ---- | ------- | ----- |
| 224.0.0.0 - 239.255.255.255 | /4 | Multicast | Not used for regular subnetting |

## CLASS E (Reserved)

| Address Range | CIDR | Purpose | Notes |
| ------------- | ---- | ------- | ----- |
| 240.0.0.0 - 255.255.255.254 | /4 | Reserved for experimental use | Not used for regular subnetting |

## Network Class Address Ranges

| Class | First Octet Range | Default Subnet Mask | Default CIDR | Leading Bits |
| ----- | ----------------- | ------------------ | ------------ | ------------ |
| A     | 1 - 127           | 255.0.0.0         | /8           | 0            |
| B     | 128 - 191         | 255.255.0.0       | /16          | 10           |
| C     | 192 - 223         | 255.255.255.0     | /24          | 110          |
| D     | 224 - 239         | N/A               | N/A          | 1110         |
| E     | 240 - 255         | N/A               | N/A          | 1111         |

## Subnet Calculation Formulas

- **Number of Subnets** = 2^n (where n = number of subnet bits)
- **Total Hosts per Subnet** = 2^h (where h = number of host bits)
- **Usable Hosts per Subnet** = 2^h - 2 (subtract 2 for network and broadcast addresses)
- **For CIDR notation /n**: Subnet bits = n - (default class bits)
