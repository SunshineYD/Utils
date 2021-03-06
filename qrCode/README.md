### 影响二维码复杂度的有几个因素：

#### 码制：
    即编码标准。但目前常见的软件都使用QR Code。其他码制有PDF417、Maxi Code等，有兴趣可以了解，但实用价值不大。

#### 信息量：
    即二维码包含的信息。二维码实际工作原理很简单，除了用于识别的区域外，其他区域黑白分别代表1和0，这些1和0包含被转化的信息和解码信息等。有兴趣可以搜索“二维码原理”。

#### 容错量：
    二维码的冗余信息。这部分信息和正常信息在一起被存储，当正常信息出现错误（如黑变白）或无法读取（如模糊、被头像覆盖）时，可以起到修正或补充的作用。QR Code的容错量有4个等级。

    因此如果想让二维码尽量小，可以精简的就是：信息量、容错量。

### 如何精简二维码信息量和容错量
    
    二维码存储的是字符，而实际使用时大多二维码存储的是URL（网址）。因此将字符录入一个网页，然后将网页的URL放入二维码，这样二维码的信息量尽管只有一个网址，但可能性是无限的。

    而网址本身也有再度精简的可能性。例如可以使用短链接做跳转：

    原网页网址是：

    https://www.kamilet.cn/art-qrcord-one/
    而用新浪的短链接可以压缩为：

    http://t.cn/E5DxJqn
    这样字符量就减少了1倍，二维码理论上就精简了一倍。而实际使用的时候，浏览器通过扫码访问到http://t.cn下的链接，浏览器会自动帮你跳转到我们的目标链接。

    而这样的链接甚至可以再次精简，比如讲刚才新浪的链接去掉http协议，改为：

    //t.cn/E5DxJqn
    二维码仍然可以正常工作（注意，可能不是所有软件都可以正常解析，因为有协议问题。但微信是可以的）。

    然后接下来，我们就可以将上面的链接生成二维码。很多API可以实现这一点，而这一步的关键在于“容错率”。

    什么是容错率？简单来说，就是二维码允许你“破坏”的比例。举例来说，我们微信加好友的二维码，中间放了头像就是典型的对二维码的破坏，但微信二维码默认的容错率是允许这个面积的破坏的。

    容错率分为4个档位：L、M、Q、H，也有用百分比来表示的，越往后代表容错率越高。容错率越高，就意味着额外加入的“就错信息”越多，二维码的信息量就越大。

    如果你可以确保二维码被污染损坏的概率极低，而且印刷质量足够好，或者二维码仅用于线上而且质量不会在传播中有重大损耗，可以选择最低的容错率。生成二维码的API均提供容错率选项，选择那个最低的可以大大减少二维码的体积（L和H档的差距超过20%）。

    总结下来采用的方法是：

    将需要存储的信息放置在一个网页上（可实现自动化）。
    将网页网址转为短链接（利用短链接网站提供的API，可实现自动化：谷歌 URL Shortener API）。
    将短链接做头部切除，然后生成二维码（利用生成二维码的API，可实现自动化：谷歌QR Code API）。
    在将生成二维码的API时，把容错率降到最低。
    保证二维码的印刷或质量，并在运输、传播过程中，减少二维码被污损的可能性。
