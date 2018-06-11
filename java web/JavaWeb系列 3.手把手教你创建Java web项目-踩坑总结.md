# JavaWeb系列 3.手把手教你创建Java web项目-踩坑总结
#javaweb

## 部分问题解决
1. 编译出错情况，需要检查编译版本，注意这个地方的编译是否一致
  ![834B1CC6-6217-4374-A75E-9179DF04C80F](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.cCQ3Lz/834B1CC6-6217-4374-A75E-9179DF04C80F.png)


2. src/main   目录下无java目录
  可创建sources文件，命名为java
  或者创建java目录，定义为sources
  ![F3022C96-6DFD-4743-A0FB-1DC5EAA686F8](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.V1iYWm/F3022C96-6DFD-4743-A0FB-1DC5EAA686F8.png)

3. 输出target目录
  编译目录输出路径
  /Users/xiaoran/personal/Collection/webActual/target/classes
  ![B8567AB0-4EA6-43E1-9876-7C3F59F460B7](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.UpwIPw/B8567AB0-4EA6-43E1-9876-7C3F59F460B7.png)

4. 无法打出class文件
  注意此处的打包应该为war包形式
  ![B12FF072-F242-4C3F-977A-8A39B5C5B4AF](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.XQRlTW/B12FF072-F242-4C3F-977A-8A39B5C5B4AF.png)

