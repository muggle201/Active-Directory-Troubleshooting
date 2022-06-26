> The Caesar Cipher
>
> Algorithm (Date,Key) --- Encrypted









# CPPolicy.inf and Policy.inf

共同点：

- 由Administrator手动创建并定义
- 在Root及Subordinate CA创建时使用
- Are defined on the signing CA where you sign and issue the certificate, not the CA where the request is generated. (?)

不同

- CAPolicy.inf必须在安装前存放于%Systemroot%
- Policy.inf则存放于requesting computer的任意位置，任意名称

.inf文件的term

- Section
- Key
- Value



### Name Constraints

通过在CA上配置的CAPolicy.inf，添加该项，以限制允许或不包含的namespace。

CA则通过subject及subject alternate name对比自己的CAPolicy.inf来做出判断。



**Common Filter**

DIRECTORYNAME = 'DC=contoso,DC=com'

DNS = .contoso.com

UPN = @contoso.com

IPADDRESS = 192.168.1.0/255.255.255.0



**Result**

- Permitted
- Not permitted
- Excluded



**Rules**

- Excluded namespaces take precedence over permitted namespaces
- 作用于subject任何名称
- 从属CA不能允许被parent CA拒绝的namespace。
- subject的name及alternate name必须全部符合constraints
- 





























