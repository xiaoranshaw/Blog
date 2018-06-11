# JavaWeb系列  1 手把手教你创建新的Java web项目
#javaweb

### 使用idea+maven+spring构建项目
archetype：原型，模版
 SNAPSHOT   快照

#### 1.创建一个新的主项目
左边栏选择Maven，选择maven的web原型（archetype），注意是红色的
![6DB09CE0-8D7C-487E-B609-3BF6261CD0FE](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.d8NC7u/6DB09CE0-8D7C-487E-B609-3BF6261CD0FE.png)  
GroupId:项目*组织*的唯一标识符。例：com.lucky  可理解为城市具体位置，北京市.朝阳区
ArtifactId:项目的唯一标识符    例：webTest   可理解为项目名字
Version:项目版本[image:EDA15E8E-A9B9-4E9F-8F80-ECB9E7D1EFAF-36887-0001EC5CFAE4F591/8B1A47C0-8FE7-4439-81F5-4C2F306F450F.png]
Next->
![8B1A47C0-8FE7-4439-81F5-4C2F306F450F](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.2jIBYF/8B1A47C0-8FE7-4439-81F5-4C2F306F450F.png)

### 2.主项目完成，可往里填充子模块
选择新建Module
![F003260C-CBF2-484C-8658-890134789D84](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.NZ1cZH/F003260C-CBF2-484C-8658-890134789D84.png)
选择原型，注意此刻选择红色圈中的原型，非绿色圈中的
![6DB09CE0-8D7C-487E-B609-3BF6261CD0FE 1](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.ZORVoR/6DB09CE0-8D7C-487E-B609-3BF6261CD0FE 1.png)
选择父类maven模块，形成依赖关系。
![AEEDB35D-F740-41D2-A7E6-7848B13BFD06](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.OVyVND/AEEDB35D-F740-41D2-A7E6-7848B13BFD06.png)
创建子模块名字
![C972C1D2-5FCF-406E-8995-877BD8CDA1D8](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.65Exyo/C972C1D2-5FCF-406E-8995-877BD8CDA1D8.png)
创建完成之后，maven会自动创建子模块的pom.xml,和更新主项目的pom.xml
![039173E8-A0EB-4D29-AE2E-A7F35141B241](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.wJWn7c/039173E8-A0EB-4D29-AE2E-A7F35141B241.png)
子模块的pom.xml依赖主项目的项目组织位置和项目名
![1F96383C-AE39-461F-9D41-0E487FF8727F](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.UFKXY3/1F96383C-AE39-461F-9D41-0E487FF8727F.png)
主项目的pom.xml中含有modules，其中包含子模块的pom.xml
![C2EB5ED5-BDAA-41EB-BEDD-634D4E9D7360](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.bD6f6g/C2EB5ED5-BDAA-41EB-BEDD-634D4E9D7360.png)