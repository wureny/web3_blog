# uniswap之AMM

## WHAT

AMM（automated market maker），自动做市商模型；被uniswap所采用，使得交易不再依赖于中心化机构，全权由市场及其参与者决定；

它本质就是由数学公式，建立一个市场交易模型，能在缺少中心化的干预下，长期满足交易者的交易需求。

## WHY

web3建立之初，要解决的最大问题之一，便是去中心化，防止权力过度集中，使得个体力量变得不足为道。

传统交易所是什么大家都知道，而web3的交易所将交易内容换成了数字资产，如NFT，稳定币等。

web3中有两种交易所，CEX（中心化交易所），DEX（去中心化交易所），前者采用托管的中心化方式，将一笔笔交易，做成订单薄（与传统方式类似），由中心化机构处理；而后者，采用非托管的去中心化方式，依托各种技术，实现交易自动化。（也有可能会做成订单薄，这与去中心化并不冲突）。

AMM被许多CEX采用，因为它满足了去中心化的要求，而且有潜力使得市场发展越来越好。

## HOW

想理解模型内涵，首先理解一下基本的交易方式。

uniswap等DEX进行的交易，是在交易池中进行的，每一种交易池，都涉及一对币之间的交换，也就是说，你想用币A换币B，那就可以去找到AB的交易池，进行交易，当然，有时候或许没有AB池子，那么你可以用A换C，再用C换B，有交易池即可。

每一个交易池，都需要存储大量AB代币，才可以顺利进行交易，这里提供的代币我们称之为交易池的流动性，提供这样的流动性的人或组织，我们称之为流动性提供者（LP）；LP必须提供等价值的代币对，例如：300u的A，300u的B ；LP会获得LP代币，作为凭证，后期凭此可以取出代币。

一个池子流动性越大，它的稳定性越强，越能实现频繁交易；为此，DEX需要激励人们提供流动性，激励的方式就是将每一笔交易所产生的服务费抽取部分给流动性提供商；当然，流动性越大，你提供相同价值的币所能获取的利益就越小。

然后，让我们来理解数学公式以及实际的对应关系，以便于理解模型。

大致有两个基本的公式：恒定和，恒定乘积。

会涉及到三个变量，x，y，k；

x对应币x在池子中的储备量，y对应币y在池子中的储备量，k对应一个常数。

还有两个重要的概念：
滑点：我在一笔交易前得知的交易价格与这笔交易实际的价格的差值。

无常损失：LP向交易池提供流动性期间，由于外部资产市场价格波动，造成的损失。

假设我们开始的时候向池子内提供价值300u的x和价值300u的y；

1. 恒定和：
    
    ## x+y=k
    
    这是一个一元一次方程，设想一下，假如交易池采取这个模型，那我往池子里存1个x，我就能获得一个y，到后面y只剩一个了，我依旧只需要一个x来换取这最后的一个y。很明显，这样的模型并没有很好地平衡供需关系。到最后，流动性会耗尽。好处是，滑点为0。所以适合稳定币的交易，因为稳定币锚定美元，脱锚的概率极小。
    
2. 恒定乘积：
    
    ## x*y=k
    
    设想一下，当我用x换y时，由于池子中x的数量即将增多，y的数量即将减少，考虑到y总价值300u，y的价格将上升，这也就意味着，我用同样价值的x所能换取的y的数量将减少；
    
    在池子拥有较大的流动性这个前提下，这个公式相比于前一个能较好地平衡供需关系，流动性几乎不会被耗尽。然而，存在滑点较大的问题。
    
    ## What’s more…
    
    1. uniswap和curve是去中心化交易赛道的两个著名项目，下图中最上方曲线是uniswap所采取的函数，stableswap的那条是curve所采用的。
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2af79706-bf25-4a47-8647-6bd69961b72a/Untitled.png)
    

我们可以发现，curve的模型函数在x与y接近等值的情况下，滑点为0，而这一特性使得curve非常适合稳定币交易，使其占据了非常大的稳定币市场。

2.关于无常损失及其问题优化方案

无常损失概念前面已经提过了，目前较为流行的解决方案是：使用预言机喂价格。
