比特币产生背景

区块链技术

区块链应用



区块链与比特币







ipfs优势   

1.内容永久存储

2.按内容搜索，速度快，星际传输

3.自己保证绝对安全，公司不需要网络安全部门

4.公司不需要备份服务器

5流量很便宜







内容寻址原理

比特币源码







什么是web3.0

钱包是入口，是互联网的身份，存储在区块链

统一一个用户钱包可登入所有app

付费了一首歌，如果平台倒闭，仍可以换平台听

可创作自己的作品



web3赛道

did

社交

域名服务 ENS

预言机 link







dapp交易量占比



crypto五个赛道

defi  gamefi nft socialfi  基建





did

<font style="color:rgb(34, 34, 34);">在 VC 的系统中，有以下 4 种参与者：</font>

1. <font style="color:rgb(34, 34, 34);">发行者（Issuer）：拥有用户数据并能开具 VC 的实体，如政府、银行、大学等机构和组织；</font>
2. <font style="color:rgb(34, 34, 34);">验证者（Verifier）：接受 VC 证书并进行认证，比如酒店入住时前台查看我们的身份证；</font>
3. <font style="color:rgb(34, 34, 34);">持有者（Holder）：通过向发行者请求、收到，最终持有 VC 的实体（用户自己）；</font>
4. <font style="color:rgb(34, 34, 34);">DID 标识注册机构（Verifiable Data</font><font style="color:rgb(34, 34, 34);"> </font><font style="color:rgb(34, 34, 34);">Registry</font><font style="color:rgb(34, 34, 34);">）：我们存贮 DID 标示和 DID 文档的地方，维护 DID 的数据库，如某条区块链、分布式账本，通过 DID 标识可以查询到对应的 DID 文档。</font>

<font style="color:rgb(34, 34, 34);">[uploading100%]</font>

<font style="color:rgb(34, 34, 34);">具体关系图</font>

1. <font style="color:rgb(34, 34, 34);">小明生成自己的 DID 地址，拥有了公钥和私钥，成为了持有者</font>
2. <font style="color:rgb(34, 34, 34);">小明的地址接收了发行者组织（具备自身 DID）颁发的证书 VC，</font>
3. <font style="color:rgb(34, 34, 34);">里面包含了证书的所有信息和小明的 DID</font>
4. <font style="color:rgb(34, 34, 34);">小明拿着 VC 给验证者看，证明自己确实得到了证书</font>
5. <font style="color:rgb(34, 34, 34);">验证者通过在 DID 标识注册机构证实发行者的 DID，确定发行者真实性，</font>
6. <font style="color:rgb(34, 34, 34);">然后再确认小明的 VC 证书真实性，最终完成认证</font>

<font style="color:rgb(34, 34, 34);"></font>

<font style="color:rgb(34, 34, 34);">根据对市场的调研和理解，在这里将它们归为三类：身份认证与管理、身份应用、底层支撑与数据标示。</font>

### <font style="color:rgb(34, 34, 34);">（一）身份认证与管理</font>
<font style="color:rgb(34, 34, 34);">这一类的项目集中在去中心化身份的认证与管理方式层面，通过一定的技术手段保证用户的 DID，并且使管理更加具有系统性，方便用户使用 DID 进行各种链上行为。</font>

**<font style="color:rgb(34, 34, 34);">1. 身份认证：</font>**

<font style="color:rgb(34, 34, 34);">BrightID 是一个去中心化的匿名社交身份网络，不收集个人隐私信息，而是通过生物识别的方式确认用户身份的唯一性，用户需要在线上与管理人员视频会议认证身份。目前，BrightIDBeta 版本 App 已上线安卓和 iOS 平台。有 6.5 万用户，兼容了 15 个 App。</font>

<font style="color:rgb(34, 34, 34);">用户下载应用程序后，可以直接进行注册，无需任何身份信息，只需要头像和名字，与朋友建立联系时，可以通过 P2P（点对点）传输来安全地分享。而经过 BrightID 验证的应用程序也可以显示在页面上。</font>

<font style="color:rgb(34, 34, 34);">目前 BrightID 使用场景包括身份识别、应用程序用户验证、活动验证 ( 空投等 )、信任及信誉构建及其他，并且还在开发 IDChain。由于项目的出色表现，它在 Gitcoin 第七轮捐赠活动中获得了 V 神的表扬。</font>

**<font style="color:rgb(34, 34, 34);">2. 身份管理工具：</font>**

**<font style="color:rgb(34, 34, 34);">ENS</font>**

<font style="color:rgb(34, 34, 34);">ENS 成立于 2017 年，以太坊基金会支持的基于以太坊的去中心化域名项目，允许用户以简化的基于文本的方式，来显示冗长的以太坊公共地址，让共享、使用和记忆地址及其他数据变得更加容易。</font>

<font style="color:rgb(34, 34, 34);">同时，ENS 还支持用户将其邮箱、</font><font style="color:rgb(34, 34, 34);">推特</font><font style="color:rgb(34, 34, 34);">、NFT 头像等与其域名相绑定，并可被第三方平台读取与展示。目前，绝大多数以太坊应用都已经支持显示 ENS 域名，也是目前使用最为广泛的身份类项目。已经有 112 万个独特域名、504 个支持项目。</font>

**<font style="color:rgb(34, 34, 34);">Spruce</font>**

<font style="color:rgb(34, 34, 34);">Spruce 是一个跨链数字身份认证系统，提供签名、共享和验证可信信息。2022 年 4 月 20 日，其完成 3400 万美元 A 轮</font><font style="color:rgb(34, 34, 34);">融资</font><font style="color:rgb(34, 34, 34);">，a16z 领投，Ethereal Ventures、Electric Capital、</font><font style="color:rgb(34, 34, 34);">Y Combinator</font><font style="color:rgb(34, 34, 34);">、Protocol Labs 等参投。</font>

<font style="color:rgb(34, 34, 34);">Spruce 与以太坊基金会、ENS 合作构建了身份验证标准化系统 Sign-In with Ethereum (EIP-4361)，支持用户直接使用他们的加密钱包与 Web2 或 Web3 应用程序连接，并控制其身份数据。</font>

<font style="color:rgb(34, 34, 34);">Spruce ID 生态由 DIDKit 、Rebase、Keylink、Credible 四部分构成：DIDKit 用于签名和验证 W3C 可验证凭证；Rebase 是用户数据凭证；Keylink 可以将现有系统帐户链接到加密密钥对；Credible 则是证件钱包。</font>

**<font style="color:rgb(34, 34, 34);">3. 身份聚合工具：</font>**

**<font style="color:rgb(34, 34, 34);">Litentry</font>**

<font style="color:rgb(34, 34, 34);">Litentry 是波卡生态的去中心化身份聚合器，支持跨多个网络链接用户身份。用户可以通过其提供的安全工具管理自己的身份，Dapp 可以获得跨不同区块链的身份所有者的实时 DID 数据。这也是 7 O’Clock Capital 的 Portfolio 之一。目前基于该项目的去中心化身份项目包括 My Crypto Profile 、Web3Go 、Polkadot Name System 、PokaSignIn 等。</font>

<font style="color:rgb(34, 34, 34);">Litentry 建立了一个三层信用计算基础设施用以支持 DID 的管理：</font>

1. <font style="color:rgb(34, 34, 34);">源数据层。身份分析员获得数据的源平台，如 Etherscan、The Graph、Onfinality 和其他数据提供商。</font>
2. <font style="color:rgb(34, 34, 34);">地址分析层。主要是作为一个提供数据分析的外部服务器，如 Nansen, Chainalysis，以及即将推出的 Litentry whitelist 等地址分析平台。</font>
3. <font style="color:rgb(34, 34, 34);">身份聚合层。Litentry 生成属于同一身份的地址关系，然后从地址分析层获取相应的地址分析数据，并进行加权计算。</font>

**<font style="color:rgb(34, 34, 34);">Unipass</font>**

<font style="color:rgb(34, 34, 34);">Unipass 是一个多链统一加密身份，即元宇宙通用护照，用户可以通过一个 Unpass ID 聚合多个社交（Web2）账号，给予用户评分、标签、展示用户的 NFT、支持基于电子邮件的社交身份恢复，以及支持基于 Token 的社群、zoom 会议、论坛访问。支持向特定 Token 持有者发送消息。</font>

**<font style="color:rgb(34, 34, 34);">.bit（前身是 DAS）</font>**

<font style="color:rgb(34, 34, 34);">.bit 是一个基于 Nervos CKB 区块链的、开源的、去中心化的</font><font style="color:rgb(34, 34, 34);">跨链</font><font style="color:rgb(34, 34, 34);">账户系统，提供全球唯一的命名系统，后缀为.bit，可用于不同场景，如加密资产转账、</font><font style="color:rgb(34, 34, 34);">域名解析</font><font style="color:rgb(34, 34, 34);">、身份认证等 。</font>

<font style="color:rgb(34, 34, 34);">任何应用程序都可以读取其中的数据，但只有用户可以决定将哪些数据写入其中。用户拥有绝对的所有权和控制权。目前注册一个账号需要支付每年 5 美金的费用，以及 0.77 美金的存储费。</font>

### <font style="color:rgb(34, 34, 34);">（二）身份应用</font>
**<font style="color:rgb(34, 34, 34);">1. 去中心化社交</font>**

**<font style="color:rgb(34, 34, 34);">CyberConnect</font>**

<font style="color:rgb(34, 34, 34);">CyberConnect 是一个多链去中心化社交图谱协议，构建了一个可拓展的标准化社交图谱模块，通过搜索引擎，能搜到具体地址的 follower、POAP 和 Galaxy 凭证。其数据通过 Ceramic 存储在 IPFS 上，为 DApp 提供通用数据层。</font>

<font style="color:rgb(34, 34, 34);">虽然社交图谱数据对所有人开放，但只有用户可以完全控制自己的社交图谱，即添加、删除和更新相关 dapp 链接。</font>

**<font style="color:rgb(34, 34, 34);">Lens</font>****<font style="color:rgb(34, 34, 34);"> </font>****<font style="color:rgb(34, 34, 34);">Protocol</font>**

<font style="color:rgb(34, 34, 34);">Lens Protocol 是 Aave 团队在 Polygon 上开发的可组合的去中心化社交图谱，具有一般的社交媒体功能，例如个人资料编辑、评论、转发帖子等。不同的是，Lens Protocol 支持 NFT，用户拥有和控制其所创作的所有内容。</font>

<font style="color:rgb(34, 34, 34);">用户通过 Profile NFTs（个人档案 NFT）查看自己历史足迹、发布的艺术内容，通过平台上关注他人获得 Follow NFT ( 跟随者 NFT)。</font>

<font style="color:rgb(34, 34, 34);">该协议也允许开发者使用模块化组件在 Lens 上任意搭建自己的社交应用，鼓励开发者开发提升产品体验的新组件，外部其他应用也可以接入 Lens，并且共享 Lens 生态的优势。</font>

**<font style="color:rgb(34, 34, 34);">2. 赏金任务：</font>**

**<font style="color:rgb(34, 34, 34);">DeWork</font>**

<font style="color:rgb(34, 34, 34);">Dework 是 Web3 原生的项目管理平台。具有 Token 支付、认证、赏金功能。目前已被多个 DAO 使用，包括 OpenDAO、AragonDAO、CityDAO 以及 ShapeshiftDAO。</font>

<font style="color:rgb(34, 34, 34);">贡献者可以建立个人 Web3 资料，从 DeWork 端可以找到合适的赏金任务，并通过完成任务获得报酬。</font>

<font style="color:rgb(34, 34, 34);">而项目方可以在上面分享项目动态，并设置一些任务和赏金，吸引更多的参与者。</font>

**<font style="color:rgb(34, 34, 34);">3. 信用 / 声誉凭证：</font>**

**<font style="color:rgb(34, 34, 34);">POAP</font>**

<font style="color:rgb(34, 34, 34);">参与证明协议（Proof of Attendance）是数字纪念品，旨在创造一种可靠的记录生活经历的新方式，为各类活动、事件的参与者办法 NFT 徽章以证明其参与了该项活动、事件，无论该事件是虚拟发生还是在现实世界中发生。是 Web3 的信用 / 声誉凭证雏形。</font>

**<font style="color:rgb(34, 34, 34);">Arcx.money</font>**

<font style="color:rgb(34, 34, 34);">Arcx.money 目前免费向用户发放 DeFi Passport，并通过处理和参考大量数据为持有者建立信用积分。信用分将通过分析持有者的以太坊地址历史活动来确定，其范围设置为 0 到 999 分，该信用分确定了协议为用户提供的</font><font style="color:rgb(34, 34, 34);">抵押率</font><font style="color:rgb(34, 34, 34);">。在申领 Passport 后，用户会受到激励，通过在多个「游戏」中最大化自己的分数来提高自己的链上声誉，这样他们就可以获得各种好处，例如以更低的抵押率进行借贷。</font>

**<font style="color:rgb(34, 34, 34);">Project Galaxy</font>**

<font style="color:rgb(34, 34, 34);">Project Galaxy 是 Web3 信用凭证，用户连接钱包后即可生成一张「银河身份证」，根据地址的历史行为被打上各类标签。「银河身份证」是用户数字身份的活动、声誉和成就记录。建设者可以基于 Galaxy 凭证定位目标受众、奖励社区、计算信用评分、建立投票系统、激励参与。支持链上、链下凭证。</font>

<font style="color:rgb(34, 34, 34);">目前 Project Galaxy 有 3000 多个 credential 标签，完成了 3000 多个基于信用的活动。</font>

### <font style="color:rgb(34, 34, 34);">（三）底层支撑和数据标识</font>
**<font style="color:rgb(34, 34, 34);">数据和公链：</font>**

**<font style="color:rgb(34, 34, 34);">Ceramic</font>**

<font style="color:rgb(34, 34, 34);">Ceramic 是一个基于 IPFS 构建的去中心化的、可跨链的、能管理动态内容数据的数据库服务。在可变性、版本控制、访问控制和可编程逻辑等层面弥补了 IPFS 的一些短板。</font>

<font style="color:rgb(34, 34, 34);">DID 用于登录 Ceramic 应用程序。每个事务或对数据流的更新都由用户（帐户）的 DID 进行身份验证。在 DID 之上，Ceramic 开发了 IDX 标准，用以聚合多种跨链数据类型关联到 DID 相关的用户数据。</font>

<font style="color:rgb(34, 34, 34);">目前已经有很多 DID 以及 Web3.0 社交平台项目在 Ceramic 上开发，如 CyberConnect，Web3.0 Twitter 的 Orbis，即时通讯平台 The Convo Space 等。</font>

**<font style="color:rgb(34, 34, 34);">Idena</font>**

<font style="color:rgb(34, 34, 34);">Idena 是第一个基于民主原则的 Proof-of-Person（个人证明）区块链。加入 Idena 需要获得已有会员的邀请码，并通过图灵测试验证身份，之后便可以成为节点、参与验证挖矿。每个挖矿节点的人都拥有相同投票权和挖矿收入，确保公正性。</font>

<font style="color:rgb(34, 34, 34);">Idena 采用定期检查点仪式——同步验证会话，来证明参与者的真实性。 验证需要解决对人类来说容易、对机器人来说困难的翻转谜题。验证节点和需要被验证的新用户需要同时解决谜题，这样能确保新用户不会多次验证自己。</font>

<font style="color:rgb(34, 34, 34);">解谜时间到后，网络会确认通过了的用户，并决定下一次集体验证的时间，人越多时间间隔越久。节点需要不断地参与验证新用户以保证自己的节点身份不过期。</font>

<font style="color:rgb(34, 34, 34);">目前 Idean 有 12892 个身份被验证，11586 个矿工，1129 个节点。合作方包括 Gitcoin, COSMOS, Amasa, Hackernoon 等。</font>

## <font style="color:rgb(34, 34, 34);">四、去中心化身份所面临的挑战和思考</font>
<font style="color:rgb(34, 34, 34);">（一）</font><font style="color:rgb(34, 34, 34);">不可能三角</font><font style="color:rgb(34, 34, 34);">难题能否被攻破?</font>

<font style="color:rgb(34, 34, 34);">对 DID 有所了解后，我们不难发现，去中心化身份也存在一个三角形难题：隐私、去中心化，抵抗 Sybil。如今的加密项目仍需要在三者之间取二舍一。</font>

<font style="color:rgb(34, 34, 34);">今天的区块链生态系统几乎普遍牺牲了对 Sybil 的抵抗来换取去中心化和隐私，如比特币、以太坊等。他们不依赖中央机构来记录身份，用户在创建钱包地址时不必披露任何个人信息，但结果是，使用这些地址作为唯一标识的项目容易受到 Sybil 攻击。</font>

<font style="color:rgb(34, 34, 34);">然而当人们试图解决 Sybil 抵抗的问题时（如 KYC），就会牺牲隐私为代价，并增加了对其他身份识别形式的依赖，而这些身份识别形式既不保护隐私也不去中心化。</font>

### <font style="color:rgb(34, 34, 34);">（二）DID 的产品形态</font>
<font style="color:rgb(34, 34, 34);">目前 DID 赛道的产品功能较为分散。未来是像 Unipass 那样朝 Web3 的入口方向发展，与钱包所融合？还是作为承上启下的枢纽，提供对使用者认证、信用评分等服务以方便上层应用运行？还是通过短期内与 Web2 平台所融合，与 Web2 共存的方式来强化可靠性和有效性？</font>

<font style="color:rgb(34, 34, 34);">目前虽然没有明确的答案，但是成为 Web3 世界的重要角色毋庸置疑。我们也期待能有更多的创新形式的展现。</font>

