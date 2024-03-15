NAME OF AFFECTED PRODUCT(S):lepton-cms 
AFFECTED AND/OR FIXED VERSION(S):7.1.0
PROBLEM TYPE:File Upload; 
Impact:Arbitrary code execution; 
DESCRIPTION:An arbitrary file upload vulnerability in LeptonCMS v7.1.0 allows authenticated attackers to execute arbitrary code via uploading a crafted PHP file.

漏洞复现：
首先去官网下载https://lepton-cms.org/english/home.php最新版本的cms。
下载好配置好之后登录进入后台:
http://localhost/LEPTON_stable_7.1.0/upload/backend/templates/index.php?leptoken=95f19723cb6cc4ac78f54z1709102857
![image](https://github.com/zzq66/cve6/assets/68541960/70a05fa7-7006-4e83-a08f-52f8a56db804)

构建一个index.php文件内容如下：
![image](https://github.com/zzq66/cve6/assets/68541960/f47207ec-7550-4ff5-8376-da41c615310a)

构建一个info.php文件，代码如下：
![image](https://github.com/zzq66/cve6/assets/68541960/f51f86fe-a20e-469c-a3f6-607822e2169f)

随后压缩这两个文件，形成一个压缩包1.zip:
![image](https://github.com/zzq66/cve6/assets/68541960/a5713ce3-3172-4427-9c6f-f92904da5a5a)

随后选择文件上传：
![image](https://github.com/zzq66/cve6/assets/68541960/a8063e38-f2e8-4068-bc75-81ad6b4b4d9c)

随后点击install，成功代码执行：
![image](https://github.com/zzq66/cve6/assets/68541960/0e7d2259-95ef-4ee4-9104-1bc48439bb20)




代码审计：
后台中upload/backend/templates/install.php对我们上传的zip文件直接进行了解压：
![image](https://github.com/zzq66/cve6/assets/68541960/2dae415b-dc89-49b0-b0b0-8b36e76d30b7)
![image](https://github.com/zzq66/cve6/assets/68541960/3f413ff6-4d57-4dfe-b3c5-359aa4d9721e)


随后检测压缩包解压后是否含有index.php文件，如果没有的话就会报错，有的话就会require($temp_unzip.'info.php');造成文件包含，因此我们将想要执行的代码放入到info.php中即可。
![image](https://github.com/zzq66/cve6/assets/68541960/661e8f62-4467-4bb3-a807-e90408b0a3ca)



缓解措施：对本模块进行修改，在包含info.php之前检测其中的恶意代码；



缓解措施：对本模块进行修改，在包含info.php之前检测其中的恶意代码；
