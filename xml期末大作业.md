# 期末大作业

55200529 宁创涛



#### 1、**作业构思**

> 首先进行dtd定义，按部就班，将所有信息进行分级；
>
> 接着编写xml信息文件，通过资料查找以及个人感悟完成即可。
>
> 对于信息的展示，通过xsl进行页面渲染，通过javascript进行动态切换。
>
> 故设计了四个xsl文件，分别进行基础信息页面渲染，电影信息页面渲染，书籍信息页面渲染，兴趣爱好信息页面渲染、景观信息页面渲染。
>
> 在基础页面xsl中，展示一些基本信息，以及为电影。书籍等信息设置一个下拉框，通过循环标题以供选择，并设置相应的JavaScript函数进行下拉框内容改变函数，以及页面。
>
> 在电影信息展示页面专注于xml中电影结点进行展示，使用一个table进行信息的展示，图片单独占一行。
>
> 书籍、兴趣爱好、景观的xsl设计思路与电影页面相似。
>
> 最后设计一个html页面，使用JavaScript进行页面初始化，加载xml文件以及基础信息xsl文件，将渲染后的页面插入到html中，并实现下拉框改变函数，获取选中的选项，找到对应的xml结点，使用相应的xsl加载结点，将生成的gtml子页面插入页面即可。



#### 2、**me.dtd**

```dtd
<!--自我介绍DTD定义-->
        <!--        根元素，包含若干基本信息-->
        <!ELEMENT 信息 (学号,班级,姓名,个人简介,电影*,书籍*,兴趣爱好*,景观*)>
        <!--        电影-->
        <!ELEMENT 电影 (名称,内容简介,图片*,评价)>
        <!--        书籍-->
        <!ELEMENT 书籍 (名称,内容简介,图片*,评价)>
        <!--        兴趣爱好-->
        <!ELEMENT 兴趣爱好 (名称,图片*)>
        <!--        景观-->
        <!ELEMENT 景观 (名称,内容简介,图片*)>

        <!ELEMENT 学号 (#PCDATA)>
        <!ELEMENT 姓名 (#PCDATA)>
        <!ELEMENT 班级 (#PCDATA)>
        <!ELEMENT 个人简介 (#PCDATA)>
        <!ELEMENT 名称 (#PCDATA)>
        <!ELEMENT 内容简介 (#PCDATA)>
        <!ELEMENT 图片 (#PCDATA)>
        <!ELEMENT 评价 (#PCDATA)>
```



#### 3、**me.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 外部引入DTD -->
<!DOCTYPE 信息 SYSTEM "me.dtd">
<!--引用XSL显示数据-->
<!--<?xml-stylesheet type="text/xsl" href="person.xsl"?>-->
<信息>
    <学号>55200529</学号>
    <班级>5</班级>
    <姓名>宁创涛</姓名>
    <个人简介>本人性格热情开朗，待人友好，为人诚实谦虚。工作勤奋，认真负责，能吃苦耐劳，尽职尽责，有耐心。具有亲和力，平易近人，善于与人沟通。学习刻苦认真，成绩优秀，品学兼优，热爱集体，曾获得“优秀学生干部”称号。积极参加课外文体活动，各种社会实践活动和兼职工作等，提高口才和人际交往能力。曾连续两年获得学院“暑期社会实践积极分子“及”优秀个人“等荣誉称号。</个人简介>
    <电影>
        <名称>泰坦尼克号</名称>
        <内容简介>1912年4月10日，号称 “世界工业史上的奇迹”的豪华客轮泰坦尼克号开始了自己的处女航，从英国的南安普顿出发驶往美国纽约。富家少女罗丝（凯特•温丝莱特）与母亲及未婚夫卡尔坐上了头等舱；另一边，放荡不羁的少年画家杰克（莱昂纳多·迪卡普里奥）也在码头的一场赌博中赢得了下等舱的船票。
            　　罗丝厌倦了上流社会虚伪的生活，不愿嫁给卡尔，打算投海自尽，被杰克救起。很快，美丽活泼的罗丝与英俊开朗的杰克相爱，杰克带罗丝参加下等舱的舞会、为她画像，二人的感情逐渐升温。
            　　1912年4月14日，星期天晚上，一个风平浪静的夜晚。泰坦尼克号撞上了冰山，“永不沉没的”泰坦尼克号面临沉船的命运，罗丝和杰克刚萌芽的爱情也将经历生死的考验。</内容简介>
        <图片>imgs/taitan1.png</图片>
        <图片>imgs/taitan2.png</图片>
        <评价>Rose可以为爱而死，最后在Jack的舍命相救及恳求下，选择了为爱而活，而被留下的那个人是痛苦的。到老奶奶的Rose站在船边上，把海洋之星扔回海中时，我想那是它最好的归宿了。灾难面前，有的人把生存的机会留给了别人，而有的人花钱占用了别人的机会。</评价>
    </电影>
    <电影>
        <名称>战狼2</名称>
        <内容简介>由于一怒杀害了强拆牺牲战友房子的恶霸，屡立功勋的冷锋（吴京 饰）受到军事法庭的判决。在押期间，亲密爱人龙小云壮烈牺牲。出狱后，冷锋辗转来到非洲，他辗转各地，只为寻找杀害小云的凶手。在此期间，冷锋逗留的国家发生叛乱，叛徒红巾军大开杀戒，血流成河。中国派出海军执行撤侨任务，期间冷锋得知有一位陈博士被困在五十五公里外的医院，而叛军则试图抓住这位博士。而从另一位华侨（于谦 饰）口中得知，杀害小云的凶手正待在这个国家。
            　　在无法得到海军支援的情况下，冷锋只身闯入硝烟四起的战场。不屈不挠的战狼，与冷酷无情的敌人展开悬殊之战……</内容简介>
        <图片>imgs/zhanlang1.png</图片>
        <图片>imgs/zhanlang2.png</图片>
        <评价>开篇长镜头惊险大气引人入胜 结合了水平不俗的快剪下实打实的真刀真枪 让人不禁热血沸腾 特别弹簧床架挡炸弹 空手接碎玻璃 弹匣割喉等帅得飞起！就算前半段铺垫节奏散漫主角光环开太大等也不怕 作为一个中国人 两个小时弥漫着中国强大得不可侵犯的氛围 还是让那颗民族自豪心砰砰砰跳个不停。</评价>
    </电影>
    <电影>
        <名称>红海行动</名称>
        <内容简介>中东国家伊维亚共和国发生政变，武装冲突不断升级。刚刚在索马里执行完解救人质任务的海军护卫舰临沂号，受命前往伊维亚执行撤侨任务。舰长高云（张涵予 饰）派出杨锐（张译 饰）率领的蛟龙突击队登陆战区，护送华侨安全撤离。谁知恐怖组织扎卡却将撤侨部队逼入交火区，一场激烈的战斗在所难免。与此同时，法籍华人记者夏楠（海清 饰）正在伊维亚追查威廉·柏森博士贩卖核原料的事实，而扎卡则突袭柏森博士所在的公司，意图抢走核原料。混战中，一名隶属柏森博士公司的中国员工成为人质。为了解救该人质，八名蛟龙队员必须潜入有150名恐怖分子的聚集点，他们用自己的信念和鲜血铸成中国军人顽强不屈的丰碑！</内容简介>
        <图片>imgs/honghai1.png</图片>
        <图片>imgs/honghai2.png</图片>
        <评价>超出预期，非常刺激，最后几场大战，血肉横飞，估计创下了近些年华语战争题材尺度之最。片尾没有强行拔高主题，或充斥个人英雄主义的色彩，整部影片都是一场接一场紧凑且高难度的任务，山地、沙漠、空降各类地形和作战任务几乎都涉及到了，狙击和营救的部分格外惊险，结尾亦干脆有力。</评价>
    </电影>
    <书籍>
        <名称>西游记</名称>
        <内容简介>中国古典四大名着之一，是一部优秀的神话小说，也是一部群众创作和文人创作相结合的作品。小说以整整七回的“大闹天宫”故事开始，把孙悟空的形象提到全书首要的地位。第八至十二回写如来说法，观音访僧，魏徵斩龙，唐僧出世等故事，交待取经的缘起。从十三回到全书结束，讲述仙界一只由仙石生出的猴子拜倒菩提门下，命名孙悟空，苦练成一身法术，却因醉酒闯下大祸，被压于五行山下。五百年后，观音向孙悟空道出自救的方法：他须随唐三藏到西方取经，作其徒弟，修成正果之日便得救。孙悟空遂紧随唐三藏上路，途中屡遇妖魔鬼怪，二人与猪八戒、沙僧等合力对付，展开一段艰辛的取西经之旅。</内容简介>
        <图片>imgs/xiyou1.png</图片>
        <图片>imgs/xiyou2.png</图片>
        <评价>这本书可谓是家喻户晓，老版的《西游记》更是让这本书成为无数人的童年。老实说，现在我吃饭还是会播西游记，看看弹幕看看剧情，不亦乐乎。
            这本书会诙谐的而非严肃的。很多地方可以看出一种特别的幽默，比如孙悟空和二郎神的对战前的对话，和黑熊精的对骂，变成高小姐戏弄猪八戒……这些内容一看文字就能想到剧情，整个阅读的过程都很乐呵。
            这是一本古代人写的书，时代局限是必然的。所以有满堂娇毕竟从容自尽这个结局。这个结局让我不舒服了很久，本该团圆美满的结局看到这么一段，心里像存了个疙瘩似的不舒服。
            有些人说《西游记》黑暗，其实里头很少详细的血腥的详细描写，你真正看进去了，是不会计较里面的黑暗描写。比如尽情杀尽了小妖，妖怪吃人等等，都没有恶趣味的细节描写，而是一笔带过。这和水浒三国都是一样的写法，似乎并不忌讳死人的描写，而是把他当成稀松平常之事。</评价>
    </书籍>
    <书籍>
        <名称>三国演义</名称>
        <内容简介>《三国演义》是中国古典四大名著之一。元末明初小说家罗贯中所著，是中国第一部长篇章回体历史演义小说。描写了从东汉末年到西晋初年之间近100年的历史风云。描写了东汉末年和整个三国时代以曹操、刘备、孙权为首的魏、蜀、吴三个政治、军事集团之间的矛盾和斗争。反映了三国时代的政治军事斗争，反映了三国时代各类社会矛盾的渗透与转化。概括了这一时代的历史巨变，塑造了一批叱咤风云的英雄人物</内容简介>
        <图片>imgs/sanguo1.png</图片>
        <图片>imgs/sanguo2.png</图片>
        <评价>三国与水浒是写在中国人骨子里的文化。哪怕没这两本总结性的书，也会其他别的小说来总结。 叙述者没有关注乱世中的民众如何，着眼点在于枭雄们的勾心斗角。他是站在统治者的角度津津乐道着宫里的故事。他指点天下，普罗大众只是他眼中的棋子。他惋惜刘备的失败，却对生灵涂炭视而不见。缺乏悲悯心。</评价>
    </书籍>
    <书籍>
        <名称>水浒传</名称>
        <内容简介>全书描写北宋末年以宋江为首的一百零八人在梁山泊聚义的故事。北宋末年，朝政腐败，官逼民反，民不得不反，上至朝廷命官，下至普通百姓，甚至是鸡鸣狗盗之徒，随着对官府幻想的一点点破灭，连生存都难以维系，最终都被逼上梁山。然而，当人民起义的壮举使好汉们士气日益高涨的时候，宋江的接受招安改变了一切，使这轰轰烈烈的水浒起义彻底地失败，使这108位水浒英雄消失在尘世间，原本风起云涌的农民起义就这样归于平静了。</内容简介>
        <图片>imgs/shuihu1.png</图片>
        <图片>imgs/shuihu2.png</图片>
        <评价>前段精彩，后面拖沓。一百单八将还未全部出场，就因人物众多而显得冗杂了；并且战争场面太多太乱，看的人审美疲劳，尤其是将星全部归位后便是一场接一场的战斗，没有层次不分详略，几乎全都浓墨重彩地铺张描绘，看得心累。后面那么多内容几个字就能概括：赢、被招安、继续赢、卸磨杀驴。 王庆的发家史反而是最有趣的部分 还是更喜欢三国里对战争的处理，侧重描绘战将与计谋安排，详略得当。 总览四大，都是各有侧重：红楼梦写高墙内膏粱子弟的钟鸣鼎食；西游记写鬼神志怪；三国写乱世各方相争的波谲云诡。只有水浒传，前期侧重写底层百姓受到的压迫与反抗，招安后又转向写征讨各方，调转重心，损害了整体结构，毕竟招安后的重点应该在于朝堂争斗，而不是两军交战。</评价>
    </书籍>
    <兴趣爱好>
        <名称>羽毛球</名称>
        <图片>imgs/yumao1.png</图片>
        <图片>imgs/yumao2.png</图片>
    </兴趣爱好>
    <兴趣爱好>
        <名称>足球</名称>
        <图片>imgs/zuqiu1.png</图片>
        <图片>imgs/zuqiu2.png</图片>
    </兴趣爱好>
    <兴趣爱好>
        <名称>跑步</名称>
        <图片>imgs/paobu1.png</图片>
        <图片>imgs/paobu2.png</图片>
    </兴趣爱好>
    <景观>
        <名称>长城</名称>
        <内容简介>长城是中国也是世界上修建时间最长、工程量最大的一项古代防御工程。自公元前 七八世纪开始，延续不断修筑了2000多年，分布于中国北部和中部的广大土地上，总计 长度达50000多千米，被称之为“上下两千多年，纵横十万余里”。如此浩大的工程不仅 在中国就是在世界上，也是绝无仅有的，因而在几百年前就与罗马斗兽场、比萨斜塔等列 为中古世界七大奇迹之一。</内容简介>
        <图片>imgs/changcheng1.png</图片>
        <图片>imgs/changcheng2.png</图片>
        <图片>changcheng3.png</图片>
        <图片>imgs/changcheng4.png</图片>
    </景观>
    <景观>
        <名称>西湖</名称>
        <内容简介>西湖具有“三面云山一面城”的地理特征。自古以来，它的美丽就名扬天下，并为众多文学艺术作品所称颂。众多古寺宝塔、亭台楼阁、桥涵堤岸等人工景观，与自然景观浑然天成，使西湖变得更加美丽。自南宋起，西湖十被认为是“天人合一”最理想、最经典的景观体现。

            西湖这一文化景观将中国景观美学的理念表现无遗，对中国的园林设计有着深远的影响。西湖文化的关键点在于仍然能够激发人们“寄情山水”的情怀。

            西湖的自然山水以“秀美”而著称，湖水与群山紧密相依。低缓的群山呈马蹄形环布在西湖的南、西、北三面，层叠而舒展，天际线柔和委婉，群山环抱中的湖水盈满平静。旖旎的湖光山色激发了中国古代文人无限的创作灵感，成为中国山水画的重要题材，也是历代诗词文学的描写对象，西湖自然山水由此承载了丰富的历史文化内涵。</内容简介>
        <图片>imgs/xihu1.png</图片>
        <图片>imgs/xihu2.png</图片>
        <图片>imgs/xihu3.png</图片>
        <图片>imgs/xihu4.png</图片>
    </景观>
    <景观>
        <名称>泰山</名称>
        <内容简介>泰山位于中国北部山东省中部的泰安市，这里是中国古代思想家孔子的故乡。泰山主峰海拔1545米，气势雄伟磅礴，享有“五岳之首”、“天下第一山”的称号。

            自古以来，中国人就崇拜泰山，有“泰山安，四海皆安”的说法。古代历朝历代不断在泰山封禅和祭祀，并且在泰山上下建庙塑神，刻石题字。古代的文人雅士更对泰山仰慕备至，纷纷前来游历，作诗记文。泰山宏大的山体上留下了20余处古建筑群，2200余处碑碣石刻。

            泰山风景以壮丽著称。重叠的山势，厚重的形体，苍松巨石的烘托，云烟的变化，使它在雄浑中兼有明丽，静穆中透着神奇。

            泰山日出是岱顶奇观之一，也是泰山的重要标志，每当云雾弥漫的清晨或傍晚，游人站在较高的山头上顺光看，就可能看到缥缈的雾幕上，呈现出一个内蓝外红的彩色光环，将整个人影或头影映在里面，好象佛像头上方五彩斑斓的光环，所以被称为“佛光”或“宝光”。泰山佛光是一种光的衍射现象，它的出现是有条件的。据记载，泰山佛光大多出现在每年6～8月份的半晴半雾的天气，而且是太阳斜照的时候。

            泰山还以石刻众多闻名天下，这些石刻有的是帝王亲自提写的，有的出自名流之手，大都文辞优美，书体高雅，制作精巧。泰山现存有石刻1696处，分为摩崖石刻和碑刻，既是记载泰山历史的重要资料，又是泰山风景中的精彩去处之一。</内容简介>
        <图片>imgs/taishan1.png</图片>
        <图片>imgs/taishan2.png</图片>
        <图片>imgs/taishan3.png</图片>
        <图片>imgs/taishan4.png</图片>
    </景观>
</信息>
```



#### 4、基础信息的htrml渲染

**person.xsl**

```xml
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method='html' version='1.0' encoding='utf-8' indent='yes'/>
    <xsl:template match="信息">
        <h3 align = "center">宁创涛的个人介绍</h3>
        <table border="1" align = "center" cellpadding="10">
<!--             姓名-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">姓名</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="姓名"/>
                </td>
            </tr>
<!--            学号-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">学号</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="学号"/>
                </td>
            </tr>
<!--            班级-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">班级</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="班级"/>
                </td>
            </tr>
<!--            个人简介-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">个人简介</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="个人简介"/>
                </td>
            </tr>
        </table>
<!--            电影-->
        <div align = "center">
            <table border="1" align = "center" cellpadding="10">
                <tr>
                    <td bgcolor="#5F9EA0" width = "70" align = "center">电影</td>
                    <td bgcolor="#E0FFFF" width = "300">
                        <select onchange="update_movie()" id="movie">
                            <option>请选择...</option>
                            <xsl:for-each select="电影">
                                <option>
                                    <xsl:value-of select="名称"/>
                                </option>
                            </xsl:for-each>
                        </select>
                    </td>
                </tr>
            </table>
            <div id="movie-content"></div>
        </div>
<!--            书籍-->
        <div align = "center">
            <table border="1" align = "center" cellpadding="10">
                <tr>
                    <td bgcolor="#5F9EA0" width = "70" align = "center">书籍</td>
                    <td bgcolor="#E0FFFF" width = "300">
                        <select onchange="update_book()" id="book">
                            <option>请选择...</option>
                            <xsl:for-each select="书籍">
                                <option>
                                    <xsl:value-of select="名称"/>
                                </option>
                            </xsl:for-each>
                        </select>
                    </td>
                </tr>
            </table>
            <div id="book-content"></div>
        </div>
<!--            兴趣爱好-->
        <div align = "center">
            <table border="1" align = "center" cellpadding="10">
                <tr>
                    <td bgcolor="#5F9EA0" width = "70" align = "center">兴趣爱好</td>
                    <td bgcolor="#E0FFFF" width = "300">
                        <select onchange="update_hobby()" id="hobby">
                            <option>请选择...</option>
                            <xsl:for-each select="兴趣爱好">
                                <option>
                                    <xsl:value-of select="名称"/>
                                </option>
                            </xsl:for-each>
                        </select>
                    </td>
                </tr>
            </table>
            <div id="hobby-content"></div>
        </div>
<!--            景观-->
        <div align = "center">
            <table border="1" align = "center" cellpadding="10">
                <tr>
                    <td bgcolor="#5F9EA0" width = "70" align = "center">景观</td>
                    <td bgcolor="#E0FFFF" width = "300">
                        <select onchange="update_view()" id="view">
                            <option>请选择...</option>
                            <xsl:for-each select="景观">
                                <option>
                                    <xsl:value-of select="名称"/>
                                </option>
                            </xsl:for-each>
                        </select>
                    </td>
                </tr>
            </table>
            <div id="view-content"></div>
        </div>
    </xsl:template>
</xsl:stylesheet>
```



#### 5、结点选择渲染页面

**movie.xsl：当下拉框选择电影时，渲染相应的xml结点**

```xml
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method='html' version='1.0' encoding='utf-8' indent='yes'/>
    <xsl:template match="电影">
        <table border="1" align = "center" cellpadding="10">
            <!-- 电影名称-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">名称</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="名称"/>
                </td>
            </tr>
            <!--内容简介-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">内容简介</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="内容简介"/>
                </td>
            </tr>
<!--            图片-->
            <xsl:for-each select="图片">
                <tr>
                    <td bgcolor="#5F9EA0" width = "70" align = "center">图片</td>
                    <td bgcolor="#E0FFFF" width = "300">
                        <img width = "300" height="200">
                            <xsl:attribute name="src">
                                <xsl:value-of select="."/>
                            </xsl:attribute>
                        </img>
                    </td>
                </tr>
            </xsl:for-each>
            <!--评价-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">评价</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="评价"/>
                </td>
            </tr>
        </table>
    </xsl:template>
</xsl:stylesheet>

```

**book.xsl：当下拉框选择书籍时，渲染相应的xml结点**

```xml
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method='html' version='1.0' encoding='utf-8' indent='yes'/>
    <xsl:template match="书籍">
        <table border="1" align = "center" cellpadding="10">
            <!-- 书籍名称-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">名称</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="名称"/>
                </td>
            </tr>
            <!--内容简介-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">内容简介</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="内容简介"/>
                </td>
            </tr>
            <!--            图片-->
            <xsl:for-each select="图片">
                <tr>
                    <td bgcolor="#5F9EA0" width = "70" align = "center">图片</td>
                    <td bgcolor="#E0FFFF" width = "300">
                        <img width = "300" height="200">
                            <xsl:attribute name="src">
                                <xsl:value-of select="."/>
                            </xsl:attribute>
                        </img>
                    </td>
                </tr>
            </xsl:for-each>            <!--评价-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">评价</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="评价"/>
                </td>
            </tr>
        </table>
    </xsl:template>
</xsl:stylesheet>

```

**hobby.xsl：当下拉框选择兴趣爱好时，渲染相应的xml结点**

```xml
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method='html' version='1.0' encoding='utf-8' indent='yes'/>
    <xsl:template match="兴趣爱好">
        <table border="1" align = "center" cellpadding="10">
            <!-- 爱好名称-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">名称</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="名称"/>
                </td>
            </tr>
            <!--            图片-->
            <xsl:for-each select="图片">
                <tr>
                    <td bgcolor="#5F9EA0" width = "70" align = "center">图片</td>
                    <td bgcolor="#E0FFFF" width = "300">
                        <img width = "300" height="200">
                            <xsl:attribute name="src">
                                <xsl:value-of select="."/>
                            </xsl:attribute>
                        </img>
                    </td>
                </tr>
            </xsl:for-each>
        </table>
    </xsl:template>
</xsl:stylesheet>
```

**view.xsl：当下拉框选择景观时，渲染相应的xml结点**

```xml
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method='html' version='1.0' encoding='utf-8' indent='yes'/>
    <xsl:template match="景观">
        <table border="1" align = "center" cellpadding="10">
            <!-- 景点名称-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">名称</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="名称"/>
                </td>
            </tr>
            <!--内容简介-->
            <tr>
                <td bgcolor="#5F9EA0" width = "70" align = "center">内容简介</td>
                <td bgcolor="#E0FFFF" width = "300">
                    <xsl:value-of select="内容简介"/>
                </td>
            </tr>
            <!--            图片-->
            <xsl:for-each select="图片">
                <tr>
                    <td bgcolor="#5F9EA0" width = "70" align = "center">图片</td>
                    <td bgcolor="#E0FFFF" width = "300">
                        <img width = "300" height="200">
                            <xsl:attribute name="src">
                                <xsl:value-of select="."/>
                            </xsl:attribute>
                        </img>
                    </td>
                </tr>
            </xsl:for-each>
        </table>
    </xsl:template>
</xsl:stylesheet>
```





#### 6、**运行结果**

**运行基础界面**

![image-20221229135111531](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135111531.png)

****

**下拉框选择选项**

![image-20221229135154199](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135154199.png)

![image-20221229135205977](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135205977.png)

![image-20221229135216665](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135216665.png)

![image-20221229135228141](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135228141.png)

**选择后切换子页面**

![image-20221229135329140](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135329140.png)

![image-20221229135346275](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135346275.png)

![image-20221229135415099](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135415099.png)

![image-20221229135431859](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135431859.png)

![image-20221229135454244](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221229135454244.png)