# 降低游戏压力的低精度模型
## 前言
### Lod模型的意义以及游戏内的工作方式

LOD模型即细节层次（Levels of Detail）模型，当一个物体覆盖屏幕区域较小时用较为粗糙的模型来描述此物体并达到节约资源/提升帧数的优化效果游戏内lod用于掉落物，展示框/石像/玩家第三人称手持等多个非第一人称场合

LOD的最终目的并不是为了在第三人称展现足够的细节，而是尽可能优化在多人/多掉落物下游戏的帧数表现。因此，我们所追求的lod效果可以适当地牺牲第一人称的结构和细节，以达成更少的块数。通常一个保证一定细节的lod建议控制在25-50个cube。

![lod_1](/gunpack/gun/lod/lod_1.png)
![lod_2](/gunpack/gun/lod/lod_2.png)

## 准备事项

建模设置与首选项参考官方的建模指南

本教程以官方的春田1873活门枪作为实例  
![lod_3](/gunpack/gun/lod/lod_3.png)  
首先在制作lod模型之前，确保你的模型资产已经转换为**自由模型**，这一步骤有助于之后的的贴图绘制环节  
![lod_4](/gunpack/gun/lod/lod_4.png)  
创建一个独立与任何组的lod组，方便之后编辑与删除其他组

## 模型创建与注意事项

切换至侧视图，基于高模的外边缘拉cube  
![lod_5](/gunpack/gun/lod/lod_5.png)  
![lod_6](/gunpack/gun/lod/lod_6.png)  
对于高模的倒角顶面，可以用一个45°的方块概括  
![lod_7](/gunpack/gun/lod/lod_7.png)  
使用图中左上角的顶点捕捉-缩放选项点击体积块的角顶点进行缩放  
![lod_8](/gunpack/gun/lod/lod_8.png)  
![lod_9](/gunpack/gun/lod/lod_9.png)  
模型的倒角基本都可以如此省略与解决  

使用一个cube概括枪管体积，所有开洞的地方在lod中都可以是实心的  
![lod_10](/gunpack/gun/lod/lod_10.png)  
扳机和护圈用一个厚度为0的方块概括，届时将用贴图绘制这些部分，注意居中  
![lod_11](/gunpack/gun/lod/lod_11.png)  
简单概括其他细节  
![lod_12](/gunpack/gun/lod/lod_12.png)  
![lod_13](/gunpack/gun/lod/lod_13.png)  
枪机为了保证效果可以使用稍复杂一点的倒角制作方法  

首先如上文，体积上增加45°的方块做为倒角面  
![lod_14](/gunpack/gun/lod/lod_14.png)  
为此体积创建贴图预览，擦去45°体积的一部分贴图，在中间补上一个体积作为整体的顶面，并可自行输入数值适当调整大小保证顶点对齐  
![lod_15](/gunpack/gun/lod/lod_15.png)  
![lod_16](/gunpack/gun/lod/lod_16.png)  
一些在高模上已优化过的简单体积可直接复制到lod，减少工作量  
![lod_17](/gunpack/gun/lod/lod_17.png)  
模型完成后建议使用Optimize插件进行面剔除优化  

全选模型后点击bb工具选项栏下的optimize  
![lod_18](/gunpack/gun/lod/lod_18.png)  
![lod_19](/gunpack/gun/lod/lod_19.png)  
并在弹出的窗口中选择两个选项，点击确定进行剔除  

> ## **注意**
> 
> optmize插件在**基岩/自由**模型中剔除具有**不稳定性**，外加lod多使用**角度特殊的体积/薄片**。剔除后请**仔细检查**模型**是否缺面少角**，如有请按下**ctrl+z回滚**。选择没被过度剔除的元素进行剔除，或是手动剔除！！！待上文步骤完成后，进入贴图的绘制

## 贴图与注意事项

全选lod模型，创建与高模相等分辨率的贴图（tacz通常使用64x做为手枪贴图，32x作为步枪，冲锋枪，狙击枪等，春田为32x），用油漆桶打好底色，擦多余的部分贴图（如上文提到的45°倒角）  

在正式开始绘制之前，强烈建议开启bb的**镜像绘制**与**锁定alpha通道**  

由于tacz的渲染剔除机制，**不建议**使用**镂空的半透明/全透明贴图**，因为会导致游戏中内面**不被渲染**。如有镂空部分的处理，请涂黑。  
![lod_20](/gunpack/gun/lod/lod_20.png)  
镜像绘制有助于减少工作量，避免重复绘制对称贴图，锁定alpha通道是为了避免上文被擦除的部分手误再画到  
![lod_21](/gunpack/gun/lod/lod_21.png)  
对于枪械的贴图，我们可以照抄高模的纹理。将吸管工具绑定至快捷键可以更快地进行绘制操作。  
![lod_22](/gunpack/gun/lod/lod_22.png)  
顶面纹理由于镜像绘画的限制会导致贴图被镜像到实际错误的角度，所以最好手动绘制；可以把视角抬到枪械上，兼顾左右的绘制。  
![lod_23](/gunpack/gun/lod/lod_23.png)  
一些比较薄的体积可直接在贴图上绘制，省去块数  
![lod_24](/gunpack/gun/lod/lod_24.png)  
底部同理  

绘制完毕查看整体效果  
![lod_25](/gunpack/gun/lod/lod_25.png)  

## 定位组的处理与游戏内实装

### 定位组继承

![lod_26](/gunpack/gun/lod/lod_26.png)  
将低模组移动与高模重合  
![lod_27](/gunpack/gun/lod/lod_27.png)  
删除多余的组  
![lod_28](/gunpack/gun/lod/lod_28.png)  
对需要的组进行保留，移动到lod组里（如muzzle_flash，通常来说scope_pos与shell组也必须保留，grip_pos等其他定位组无须保留，因为muzzle与grip配件并无lod，出于优化考虑无须在第三人称渲染这些部件。由于春田没有程序抛壳以及瞄具设定，故仅保留用于渲染开火的muzzle_flash）  
![lod_29](/gunpack/gun/lod/lod_29.png)  
Positioning组控制掉落物位置，第三人称手持位置展示框位置，与高模完全一致即可。  

最后，删除多余组和高模的贴图，并重命名文件与贴图  
![lod_30](/gunpack/gun/lod/lod_30.png)  
将模型转为基岩版格式，准备导出到枪包内  

### 游戏实装

将模型导出到枪包路径\assets\tacz\geo_models\gun\lod内  

![lod_31](/gunpack/gun/lod/lod_31.png)  
贴图导出到枪包路径\assets\tacz\textures\gun\lod内  
![lod_32](/gunpack/gun/lod/lod_32.png)  
打开低模对应武器的display文件，检查调用路径是否正确  

官方包内高模已提前写入调用路径，如果你的display里没有，请参考下图补充  
![lod_33](/gunpack/gun/lod/lod_33.png)  
进入游戏，检查lod是否被正确载入以及表现效果  
![lod_34](/gunpack/gun/lod/lod_34.png)  
着重观察在第三人称下武器的开火/抛壳是否正常，安装配件检查是否显示且位置是否正确。  






