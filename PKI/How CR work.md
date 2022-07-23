# How Certificate Revocation Works



## 基本Certificate Chain验证

1. 使用本地缓存的certificates建立所有可能的certificate chain。若并非以root /self-signed certificate结尾，则CryptoAPI选择一条最佳chain并尝试通过AIA extension获取issuer certificate以完成chain。该过程将一直重复，直至获取到root/self-signed certificate。
2. 对每一个以root/self-signed certificate结束的certificate chain进行revocation checking。
3. Revocation checking对certificate chain中所有证书检查。



### Chain building

- Provided by the application
- Ctypt32.dll缓存的一组certificates
- 本地certificates，包含通过group policy发布的certificate。
- CrptoAPI缓存在本地磁盘的
- 通过AIA extension标注的位置，从网络获取。



### Certificate Validation

1. CryptoAPI确认证书是否属于Untrusted certificate store。若存在直接disallowed。
2. Stapled OCSP response
3. 若一个CRL符合issuer name，则使用该CRL
4. 若stapled response或CRL不存在，则CryptoAPI必须通过URL来获取并决定该证书的revocation状态。
5. URLs for OCSP和CDP以下列顺序：
   - OCSP URLs from group policy
   - ORCS URLS from AIA extension
   - CRL URLs from CDP extension



以下两种情况，CryptoAPI可能在检索OCSP URLs之前检索CRLs

- Cached OCSP responses以超过设定值，该值由group policy设置
- Group policy配置了CRLs优先于OCSP

> 仅评估完整的Certificate chain



## 确认Revocation信息

若发现证书被revoked，那么revocation date则通过比较当前时间与CRL或OCSP response中的RevocationDate来确认。



