# Chinese-NewWordRecognition
中文新词发现/中文专业词库构建
##### 主要流程：
1. 根据ngrams的设置，产生所有可能的词片段，并统计每个片段的频数。
2. 根据频数计算每个片段的凝固度，不同长度的片段设置不同的阈值，根据阈值抛弃一定的片段。此时得到一个集合G，包含凝固度较高的片段。
3. 使用集合G中的片段作为词库，切分文本，并统计切分后产生的片段的频数。根据阈值抛弃一定频数较低的片段。此时得到一个几个W，包含文本所有可能的词。注意，此时的W已经完全和G不一样了，W是大于G的，切分会产生很多新的片段。
4. 使用集合G中的片段作为词库，切分文本，并统计切分后产生的片段的频数。根据阈值抛弃一定频数较低的片段。此时得到一个几个W，包含文本所有可能的词。注意，此时的W已经完全和G不一样了，W是大于G的，切分会产生很多新的片段。这里的切分规则也是特殊的：为了更大限度的保存可能的新词，采取宁放过，勿切错的原则，即：切分时，只要一个片段的一个 。这里的切分规则也是特殊的：为了更大限度的保存可能的新词，采取宁放过，勿切错的原则，即：切分时，只要一个片段的一个子串存在G中，这个片段就不切分，比如“我们俩”中的“我们”在G中，“我们俩”就不切分。采用这种原则，可以保存更多的分词可能。当然，这样做也可能导致更多的错误分词产生。后面可采用方法一定程度上删除这些错误。
5. 边界熵计算。之所以把边界熵放到最后计算，是因为边界熵计算量过大，放在最后可以减少一定的计算量。统计集合H中所有片段的在文本中所有可能的左右字，计算每个片段的边界熵，根据阈值删除一定的片段。得到集合E，包含筛选出的文本中可能存在的词语。
6. 发现领域新词。这里读取一个较大的通用词库C，包含可能的通用常用词。使用集合E减去集合C，最终得到该领域的专业词汇F。



##### 运行效果
使用一个小规模的微博语料库作为测试样本，文件大小约13.7M，包含411109行，3350560个字符。
```
回复@南极JaronNan:I have other shows so I cannot continue on the #saythewordstour# . But @曲婉婷Wanting a tour lasts until the end of March in North America ! Go see it !//@南极JaronNan:So that means your show has come to an end so far?
This is the #TryingToMinimizeMyCoffeeIntake this week look... #poutyface #selfie  http://t.cn/8soQlCD
回复@EVA_LuckyCharm:[害羞]//@EVA_LuckyCharm:[爱你]//@CodyKarey:回复@停转的牧马:I was! But dat coffee!!!! [哈哈]//@停转的牧马:you look so tired[困]
回复@wen雅茹想去见Taylor:[嘻嘻]//@wen雅茹想去见Taylor:[熊猫][熊猫][熊猫][心]
回复@邓小导:Ni Hao!//@邓小导:why,no@me!!!//@CodyKarey:回复@AngelineTancherla陈奕琪:Ni Hao! [嘻嘻]
回复@停转的牧马:I was! But dat coffee!!!! [哈哈]//@停转的牧马:you look so tired[困]
回复@CoMe_SunShinE:Thanks! People are so friendly on Weibo!//@CoMe_SunShinE:hey，man, welcome to weblog~
I like this new haircut... Fresh cut, #NewMan!  http://t.cn/8s9Mt7b
回复@南极JaronNan:I agree! Beautiful and elegant woman [呵呵]...but young at heart and so much fun! @曲婉婷Wanting
#ottawa# tonight! See you there :)#saythewordstour# @曲婉婷Wanting [酷] 我在:http://t.cn/8FsPuqg
回复@Mangena826:Me! :p//@Mangena826:Who is kid？
Amazing night! Thank you!
点击这个链接来听我的整张专辑! i.xiami.com/codykarey ,  I want to show you my music! Click the link to listen to my whole album! #love##codykarey#  我在:http://t.cn/8FI4Je5
```
2gram, 3gram, 4gram的阈值分别设为：100， 300， 300
```
QQ,3.7223300939932797
CEO,3.5930493250144435
APP,3.575600809108179
傻逼,3.306952018318031
移动互联网,3.247403216222488
产品经理,3.1998839573644795
用户体验,3.1590249170989746
XX,3.099107168841048
B2C,3.086834460580618
社交网络,3.083592896188006
DIY,3.017968258317022
公益组织,2.940995354750372
O2O,2.896922080340242
UGC,2.875837087236039
新浪微博,2.856887658048256
社会化媒体,2.8361741471043693
装逼,2.8323551988856295
4S店,2.8117286875338543
脖友,2.67329998301815
copy,2.6499487930019865
养狗,2.611345912365964
僵尸粉,2.6008438596813606
约炮,2.5817686297648366
只剩下,2.570859383012243
XXX,2.5625354503536792
```

