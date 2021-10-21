# 模型扩展

## 连续模型扩展

### 敏感性分析

一般来说，模型参数的后验分布可以高估或低估“真实”后验不确定性的不同方面。后验分布通常高估了不确定性，因为人们通常不会在模型中包含一个人的所有实质性知识；因此，根据一个人的实质性知识检查模型的效用。另一方面，后验分布在两个方面低估了不确定性：首先，假设模型几乎肯定是错误的，因此需要对观察到的数据进行后验模型检查——其次，其他合理的模型可以同样很好地拟合观察到的数据，因此需要进行敏感性分析。在本节中，我们考虑由于存在合理的替代模型而导致的后验推断的不确定性，并讨论如何扩展模型以解释这种不确定性。替代模型的不同之处在于先验分布的规范、似然性的规范或两者都有。模型检查和敏感性分析相辅相成：在进行敏感性分析时，只需要考虑符合实际知识和观测数据的模型。
	 
敏感性分析的基本方法是对同一问题拟合多个概率模型。通常可以通过用代表实质性先验知识的适当分布替换不适当的先验分布来避免敏感性分析中的意外。此外，不同的问题受模型变化的影响不同。自然地，与关于均值或极端分位数的推断相比，关于后验分布中位数的后验推断通常对模型的变化更不敏感。类似地，对与观察数据最相似的量的预测推断是最可靠的；例如，在回归模型中，内插对线性假设的敏感度通常低于外推。有时可以使用“稳健”模型进行敏感性分析，以确保异常观察（或分层模型中更大的分析单元）不会对推论产生不当影响。典型的例子是使用 t 分布代替正态分布（用于抽样或人口分布）。这样的模型可能很有用，但需要更多的计算工作。  

<br/>

### 向模型添加参数

扩展模型有几个可能的原因:  

1. 如果模型在某些重要方面不适合数据或先验知识，则应该以某种方式对其进行更改，可能通过添加足够的新参数来实现更好的拟合。  
2. 如果建模假设有问题或没有真正的理由，可以扩大模型的类别（例如，用 t 替换正常值）。  
3. 如果考虑两个不同的模型 p1(y, θ) 和 p2(y, θ)，可以使用连续参数化将它们组合成一个更大的模型，将原始模型作为特殊情况包括在内。例如，第 5 章中的 SAT 辅导的层次模型是完全汇集(complete-pooling) (τ = 0) 和非汇集(no-pooling) (τ = ∞) 模型的连续推广。  
4. 可以扩展模型以包含新数据；例如，先前单独分析的实验可以插入到分层人口模型中。另一个常见的例子是将 y|x 的回归模型扩展为 (x, y) 的多元模型，以便对 x 中的缺失数据进行建模。  

<br/>

模型扩展的所有这些应用都具有相同的数学结构：旧模型 p(y,θ) 嵌入或替换为新模型 p(y,θ,φ) 或更一般的 p(y,y*,θ,φ)，其中y*表示添加的数据。  
新参数 φ 和旧模型参数 θ 的联合后验分布为：   

$$
p(θ, φ|y,y*) \propto p(φ)p(θ|φ)p(y,y*|θ,φ)
$$

条件先验分布p(θ|φ) 和似然性p(y, y∗|θ,φ) 由扩展族确定。φ的边际分布是通过对θ 求平均得到的：  

$$
p(φ|y,y*)\propto p(φ)\int [p(θ|φ)p(y,y*|θ,φ)dθ] \tag{7.17}
$$

在贝叶斯模型的任何扩展中，必须指定一组先验分布 p(θ|φ) 来替换旧的 p(θ)，以及超参数上的超先验分布 p(φ)。这两项任务通常都需要思考，尤其是对于非信息性先验分布。  

<br/>

### 数据分析中模型选择的会计处理

我们通常只有在大量数据分析后才能构建模型的最终形式，这会导致与多重比较和预测误差估计等经典问题相关的担忧。如第 4.5 节所述，多重比较的贝叶斯处理使用分层建模，同时估计所有可能比较的联合分布并适当缩小这些分布（例如，在八学派的分析中，$θ_{j} $ 都缩小到 µ，因此差异$θ_{j} $−$θ_{k} $ 会自动向 0 缩小）。尽管如此，仍会出现一些潜在的问题，例如可能对单个数据集进行多次分析以找到最有力的结论。 这对于所有应用的统计方法都是一种危险，并且只能通过贝叶斯尝试将所有不确定性来源纳入模型中来部分缓解。   

<br/>

### 预测因子的选择和信息的结合

在回归问题中，通常有许多不同的看似合理的方法来建立模型，这些不同的模型可以给出截然不同的答案。以预测变量的形式将现有信息放在一起几乎总是观察性研究中的一个问题，并且可以被视为模型规范问题。即使只有少数预测变量可用，我们也可以在可能的转换和交互中进行选择。

我们更喜欢在回归中包含尽可能多的预测变量，然后将它们缩放并分批到方差分析结构中，以便在某种程度上考虑它们而不是离散的输入或输出。即便如此，在选择要包含在层次模型本身中的变量时也必须做出选择。用于离散模型平均的贝叶斯方法在这里可能会有所帮助，尽管我们还没有在我们自己的研究中使用这种方法。

在观察性研究中为因果推断建立回归模型时，会出现一个相关且更基本的问题。在此，实质性背景中变量之间的关系是相关的，如在主要分层方法中，其中，在构建模型之后，需要额外的分析来计算相关因果估计，这些估计通常与回归系数不同。

<br/>

### 替代模型公式

我们经常发现向模型添加参数会使其更加灵活。例如，在正常模型中，我们更喜欢估计方差参数而不是将其设置为预先选择的值。在下一阶段，t 模型比普通模型更灵活，这已被证明在许多应用中具有实际意义。但为什么不这样做呢？在准确性和便利性之间总有一个平衡点。预测模型检查可以揭示严重的模型不匹配，但我们还没有很好的一般原则来证明我们选择基本模型的合理性。随着分层模型的计算变得越来越常规，我们可能会开始使用更精细的模型作为默认值。很难为模型选择提供适当的一般建议；与模型构建一样，需要科学判断，方法必须因环境而异。

对于模型检查和敏感性分析，我们推荐的方法是检查实质性重要参数和预测量的后验分布。然后我们将后验分布和后验预测与实质性知识（包括观测数据）进行比较，并注意预测失败的地方。应该使用差异来建议模型可能的扩展，可能就像将真实的先验信息放入先验分布或在回归中添加非线性项等参数一样简单，或者可能需要一些实质性的重新思考，如总统选举模型中南方各州的预测差，如下：
<div align=center><img src="https://github.com/Redyhat/2021BayesianCourse/blob/0e2f5414374735d0f2d13614fe12c2b8aa2e4584/figure/xjx_figure1.png",width=100></div>

  
有时模型的假设比显而易见的要强。例如，具有许多预测变量和系数的平稳先验分布的回归往往会高估系数之间的变化，就像对八所学校的独立估计过于分散一样。如果我们发现模型不适合其预期目的，我们有义务寻找适合的新模型；分析很少（如果有的话）仅仅通过拒绝某个模型来完成。  

如果敏感性分析揭示了问题，基本的解决方案是将其他似是而非的模型包含在先验规范中，从而形成反映模型规范中不确定性的后验推断，或者简单地报告对手头数据无法检验的假设的敏感性。有时必须得出结论，就实际目的而言，可用数据无法有效回答某些问题。在其他情况下，可以添加信息来约束模型，以允许进行有用的推理；下面给出了一个来自非正态总体的简单随机样本的例子，其中感兴趣的数量是总体总数。  

<br/>

## 隐式假设和模型扩展：举例

尽管我们尽了最大努力来包含信息，但所有模型都是近似的。因此，检查模型对数据和先验假设的拟合总是很重要的。出于模型评估的目的，我们可以将贝叶斯数据分析的推理步骤视为一种复杂的方法，以这种方式探索所提出模型的所有含义，以便将这些含义与观察到的数据和未包含的其他知识进行比较。例如，第 6.4 节说明了针对心理学研究中两个不同问题的数据拟合模型的图形预测检查。在每种情况下，拟合模型都捕获了数据的一般模式，但遗漏了一些关键特征。在第二个示例中，发现模型失败会导致模型改进——患者和症状参数的混合分布更好地拟合数据，如下图所示。  

<div align=center><img src="https://github.com/Redyhat/2021BayesianCourse/blob/0e2f5414374735d0f2d13614fe12c2b8aa2e4584/figure/xjx_figure2.png",width = 100></div>

后验推论通常可以用图形来概括。对于简单问题或一维或二维总结，我们可以绘制后验模拟的直方图或散点图。对于较大的问题，汇总图很有用。几个独立推论的图有助于总结迄今为止的结果并建议未来的模型改进。  

在检查模型时，必须牢记它的用途。例如，第 1.6 节中足球比分的正态模型准确地预测了获胜的概率，但对比赛完全打平的概率给出了糟糕的预测。  

我们还应该知道自动贝叶斯推理的局限性。即使模型很好地拟合了观察到的数据，也可能对某些感兴趣的数量产生不良的推断。  

看到当模型未经模型检查时可能出现的陷阱，令人惊讶且具启发性。  
  
### **例子：使用转换的正态模型估计简单随机抽样下的总体总数**

<div align=center><img src="https://github.com/Redyhat/2021BayesianCourse/blob/0e2f5414374735d0f2d13614fe12c2b8aa2e4584/figure/xjx_table1.png",width =100></div>

我们考虑从 n = 100 的简单随机样本中估计 1960 年纽约州 N = 804 个市镇的总人口的问题——一个人为的例子，但它说明了模型检查在避免严重错误推理方面的作用。表 7.2 总结了这次“调查”的总体以及两个简单的随机样本（这是第一个也是唯一选择的样本）。 由于了解总体情况，两个样本都显得特别不典型； 根据所提供的汇总统计数据，样本 1 代表总体，而样本 2 的值太大。 因此，乍一看，估计总体总数似乎很简单，可能高估了第二个样本的总数。  

<br/>

### 样本 1：初步分析  
我们首先尝试从样本 1 估计总体总数，假设总体中的 N 个值来自 N(µ, $σ_{2}$ ) 超总体，在 (µ, log σ) 上具有统一的先验密度。 为了使用第 8 章中更正式引入的符号，我们希望估计有限人口数量：
$$
y_{total} = N\bar{y} =n\bar{y}_{obs}  +(N-n)\bar{y}_{mis} \tag{7.18}
$$
其中,$y_{obs}$是 100 个观察到的城市的平均值，$y_{mis}$ 是其他 704 个城市的平均值。使用来自表 7.2 第二列的数据和列表中的 t 分布，我们获得了 $y_{total}$ 的以下 95% 后验区间：[−5.4 × $10^{6} $ , 37.0 × $10^{6} $ ]。检查这个 95% 区间的实际人员可能会发现上限很有用，并简单地用样本中的总数替换下限，因为总体中的总数不能更少。此过程给出了 [2.0 ×$10^{6} $, 37.0 × $10^{6} $ ] 的 95% 区间估计。  

当然，适度智能地使用统计模型应该会产生更好的答案，因为正如我们在表 7.2 中看到的那样，总体和样本 1 都远非正常，而标准区间最适合正常总体。此外，提前知道总体中的所有值都是正数。  

我们在假设完整数据中的 N = 804 个值遵循对数正态分布的假设下重复上述分析：log $y_{i}$ ∼ N(µ, $σ^{2}$ )，在 (µ, log σ) 上具有均匀的先验分布。$y_{total}$的后验推断以通常的方式执行：从它们的后验（正态逆 $χ^{2}$ ）分布中绘制 (µ, σ)，然后从预测分布中绘制 $y_{mis}$|µ, σ，最后从 (7.18) 中计算$y_{total}$. 基于 100 次模拟绘制，$y_{total}$的 95% 区间为 [5.4×$10^{6} $ , 9.9×$10^{6} $ ]。这个区间比原来的区间更窄，乍一看似乎有所改进。  

<br/>

### 示例 1：检查对数正态模型 
我们的主要原则之一是检查模型的拟合。 因为我们对总体总数 $y_{total}$感兴趣，所以我们使用样本中的总数 $ T(y_{obs}) = {\textstyle \sum_{i=1}^{n}} { y_{obs}}_{i}$ 作为检验统计量来应用后验预测检查。 使用我们从对数正态模型下的后验分布抽取 (μ,  $σ^{2}$) 的 S = 100 个样本，我们获得 S 个独立复制数据集$y_{obs}^{rep}$的后验预测模拟，并计算 $T(y_{obs}^{rep}) = {\textstyle \sum_{i=1}^{n}} {y_{obs}^{rep}}_{i} $ 为每个。 结果是，对于这个预测量，对数正态模型是不可接受的：所有 S = 100 模拟值都低于样本中的实际总数 1966745。  

<br/>

### 示例 1：扩展分析 
超越对城市规模的对数正态模型的自然概括是幂变换正态族，它向模型添加了一个额外的参数 φ； 有关详细信息，请参阅附录上的 (7.19)。 值 φ = 1 和 0 分别对应于未变换的正态模型和对数正态模型，其他值对应于其他变换。  

要将转换后的正态族拟合到数据$y_{obs}$，最简单的计算方法是将正态模型拟合到几个 φ 值处的转换数据，然后计算 φ 的边际后验密度。使用来自样本 1 的数据，φ 的边际后验密度在值 -1/8 附近强烈达到峰值（假设 φ 的先验分布均匀，鉴于相对信息可能性，这是合理的）。基于扩展模型下的 100 个模拟值，$y_{total}$ 的 95% 区间为 [5.8 ×$10^{6} $ , 31.8 × $10^{6} $ ]。关于后验预测检查，样本总数的 100 个模拟重复中有 15 个大于实际样本总数；该模型在这个意义上非常适合。  

也许我们已经学会了如何成功地应用贝叶斯方法来估计具有此类数据的总体总数：使用幂变换族并通过模拟绘图总结推理。但我们并没有对这个猜想进行严格的检验。我们从对数转换开始，并获得了最初看起来不错的推论，但我们看到后验预测检查表明模型在预测样本总数方面缺乏拟合。然后，我们扩大了变换族并在更大的模型下进行了推理（或者，在这种情况下，等效地，找到了最合适的变换，因为变换能力是由数据如此精确地估计的）。扩展程序似乎在 95% 的区间是合理的意义上起作用；此外，对样本总数的事后预测检查是可以接受的。为了检查这个扩展程序，我们在第二个随机样本 100 上进行尝试。  

<br/>

### 样本 2  
对来自第二个样本的总体总数的基于标准正态的推断产生 [−3.4 × $10^{6} $ , 65.3 × $10^{6} $ ] 的 95% 区间。用样本总数代替下限给出[3.9 × $10^{6} $ , 65.3 × $10^{6} $ ] 的宽区间。按照样本 1 中使用的步骤，将样本 2 数据建模为对数正态导致 $y_{total}$  的 95% 区间为 [8.2 × $10^{6} $ , 19.6 × $10^{6} $ ]。对数正态推断是严格的。然而，在使用对数正态模型对样本 2 进行后验预测检查时，样本总数的 100 次模拟中没有一个与观察到的样本总数一样大，因此我们再次发现该模型不适合估计总体总数。根据我们对样本 1 的经验以及两个样本在对数正态模型下的后验预测检查，我们不应相信对数正态区间，而应考虑一般幂族，其中包括对数正态作为特例。对于样本 2，功率参数 φ 的边际后验分布在 -1/4 处达到强烈峰值。后验预测检查生成 100 个样本总数中的 48 个大于观察到的总数——没有任何问题的迹象，至少如果我们不检查正在生成的特定值。  

在这个例子中，我们有幸知道正确的值（实际总人口为 1380 万），从这个角度来看，对权力家族下的人口总数的推断是很糟糕的：例如，100 的中位数 $y_{total}$ 的生成值为 57 × $10^{7} $，第 97 个值为 14 × $10^{15} $，生成的最大值为 12 × $10^{17} $ 。  

<br/>

### 需要指定关键的先验信息
到底是怎么回事？为什么样本2的总体推断在一个拟合较好的模型(即假设y−1/4 i为正态分布)下比在一个拟合较差的模型(即假设log$y_{i}$为正态分布)下更不现实？  
本例中推论的问题不是模型无法拟合数据，而是数据固有的无法区分对估计总体总数$y_{total}$ 具有不同影响的替代模型。$y_{total}$ 的估计在很大程度上取决于城市大小分布的上限，但是当我们拟合幂族等模型时，这些模型的右尾（尤其是超过 99.5% 分位数）正受到管理变化的显着影响通过模型与数据主体的拟合（介于 0.5% 和 99.5% 分位数之间）。$y_{total}$ 的推论实际上严重依赖于超过对应于最大观察到的${ y_{obs}}_{i}$的分位数的尾部行为。为了估计总数（或平均值），我们不仅需要一个合理拟合观测数据的模型，而且我们还需要一个能够提供超出数据区域的真实外推的模型。对于此类推断，我们必须依赖于先前的假设，例如指定最大可能的自治市规模。

更明确地说，对于我们的两个样本，幂族的三个参数基本上足以为观测数据提供合理的拟合。但是为了从大小为 100 的简单随机样本中获得对纽约州人口的现实推论，我们必须限制大城市的分布。事实上，我们被来自样本 2 的样本总数的后验模拟的特定值警告了，其中复制样本总数的 100 个模拟中有 10 个大于 3 亿！

用于批判幂变换正态模型的实质性知识也可用于改进模型。 假设我们知道没有一个城市的人口超过 5 × $10^{6} $。 为了将这些信息包含在模型中，我们简单地以与之前相同的方式绘制后验模拟，但截断市政规模以使其低于该上限。 由此产生的对总人口规模的后验推断是合理的。 对于这两个样本，幂族下$y_{total}$ 的推论比未截断模型的推论更严格并且是现实的。 样本 1 和样本 2 下的 95% 区间分别为 [6 × $10^{6} $ , 20 ×$10^{6} $ ] 和 [10 × $10^{6} $ , 34 × $10^{6} $ ]。 顺便说一下，真实的总体总数是 13.7×$10^{6} $（见表 7.2），它包含在两个区间中。  
<br/>

### 为什么未转换的正态模型在估计人口总数时效果相当好？ 
基于$y_{i}$的简单未变换正态模型的$y_{total}$ 推论并不可怕，即使没有提供城市规模的上限。为什么？在正态模型下对$y_{total}$ 的估计基本上仅基于${\bar{y}}_{obs} $ 的假设正态抽样分布和 ${s_{obs}}^{2} $的相应 $χ^{2} $抽样分布。为了相信这些抽样分布近似有效，我们需要应用中心极限定理，我们通过隐式限制 $y_{i}$的分布的上尾足以使近似正态性适用于 100 的样本大小来实现。这并不是建议我们为明显的非正态数据推荐未转换的正态模型；在此处考虑的示例中，有界幂变换族可以更有效地利用数据。此外，未转换的正态模型对总体中位数等估计值的推断非常差。一般来说，限制$y_{i}$的大值的贝叶斯分析必须明确这样做。  
<br/>

### 精心设计的样本或可靠的问题无需强大的先验信息
在实践中常规地估计总数不需要广泛的建模和模拟。优秀的调查从业者都知道，简单的随机样本不是用于估计高度偏斜总体中的总数的好调查设计。如果分层变量可用，人们更愿意对大城市进行过度抽样（例如，对纽约市的所有五个行政区、大部分城市和较小比例的城镇进行抽样）。  
<br/>

### 人口中位数的推断
然而，不应忽视的是，我们抽取的简单随机样本虽然对于估计总体总数并不理想，但在没有强加先验限制的情况下回答许多问题是令人满意的。  

例如，考虑对 804 个城市的中位数规模进行推断。使用来自样本 1 的数据，模拟 95% 后验区间在三个模型下的中位数城市规模：(a) 对数正态，(b) 幂变换的正常家庭，和 (c) 以 5 ×$10^{6} $倍截断的幂变换的正常家庭分别是 [1800, 3000]、[1600, 2700] 和 [1600, 2700]。基于样本 2 的可比较区间为 [1700, 3600]、[1300, 2400] 和 [1200, 2400]。一般来说，更好的模型往往会给出更好的答案，但对于手头数据稳健的问题，例如从我们的大小为 100 的简单随机样本中估计中位数，效果相当微弱。对于此类问题，先验约束并不是非常关键，即使是相对不灵活的模型也能提供令人满意的答案。此外，对于所有这些模型（但对于未转换的正态模型），样本中位数的后验预测检查看起来很好——观察到的样本中位数接近模拟样本中位数分布的中间。  
<br/>

### 结论：

我们从这个例子中学到了什么一般经验？ 前两个消息特定于示例，并解决了覆盖真实人口总数的推断的准确性。  

1. 对数正态模型可能对总体总数产生不准确的推论，即使它看起来与观察数据相当吻合。  

2. 将对数正态族扩展到更大、更适合的模型，例如幂变换族，可能会导致对总体的推断不太现实。  

   这两点不是对对数正态分布或幂变换的批评。 相反，当使用未经后验预测检查（用于与感兴趣的估计相关的测试变量）和现实检查的模型时，它们会提供警告。 在这种情况下，“更适合数据意味着更好的模型，进而意味着更好的现实世界答案”的幼稚说法不一定正确。 统计答案依赖于先验假设和数据，而更好的现实世界答案通常需要包含更现实的先验假设（例如市政规模的界限）并提供更好的数据拟合模型。 这种评论自然会导致包含前两点的一般信息。  

3. 一般而言，推论可能对观测数据无法解决的总体中值的潜在分布特征很敏感。 因此，为了获得好的统计答案，我们不仅需要拟合观察数据的模型，还需要：  

   (a) 这些模型的灵活性，以允许规范观察到的数据没有充分解决的现实潜在特征，例如分布极端尾部的行为，或  

   (b) 对于所收集的数据类型而言，问题是可靠的，即人口值的所有相关潜在特征都由观察值充分解决。  

寻找满足 (a) 的模型是比寻找满足 (b) 的问题更通用的方法，因为统计学家经常面临需要某种答案的困难问题，并且没有摆出简单（即稳健）的奢侈 问题在他们的位置。 例如，出于环境原因，使用周围地理区域的土壤样本估计制造工厂排放的污染物总量可能很重要，或者，为了编制医疗保险计划的预算，可能 需要从患者样本中估计医疗费用总额。 这些问题本质上是不可靠的，因为它们的答案取决于基础分布的极端尾部的行为。 估计更可靠的人口特征，例如土壤样本中污染物的中位数或患者的医疗费用中位数，并不能解决此类示例中的基本问题。  
相关的推理工具，无论是贝叶斯还是非贝叶斯，都不能没有假设。 贝叶斯推理的稳健性是数据、先验知识和正在考虑的问题的共同属性。 对于许多问题，统计学家可能能够定义所研究的问题，以便得到可靠的答案。 然而，有时，实际的、重要的问题不可避免地不可靠，推论对手头数据无法解决的假设很敏感，然后一个好的贝叶斯分析表达了这种敏感性。
<br/>

## 附录

1.幂变换的正态模型：对于全正数据，正态分布族的自然扩展是通过幂变换，用于各种环境，包括回归模型。 为简单起见，考虑单变量数据 y = ($y_{1}$, . . , $y_{n}$)，我们希望将其建模为独立且在变换后通常分布相同的数据。 Box 和 Cox (1964) 提出了模型 $y_{i}^{(φ)}  $∼ N(µ, $σ^{2}$ )，其中
$$
\begin{equation}
y_{i}^{(φ)}=\left\{
\begin{aligned}
(y_{i}^{(φ)}-1)/φ&& for φ\ne 0 \\
logy_{i} && for φ=0 \\
\end{aligned}
\right.
\end{equation} \tag{7.19}
$$
$y_{i}^{(φ)} $方面的参数化允许包含对数作为特殊情况的连续幂变换族。 要执行贝叶斯推理，必须为参数 (μ, σ, φ) 设置先验分布。
<br/>

## 参考文献
有许多应用贝叶斯分析的例子，其中已经检查了对模型的敏感性，例如 Racine 等人。 (1986)、Weiss (1994) 和 Smith、Spiegelhalter 和 Thomas (1995)。 Calvin 和 Sedransk (1991) 提供了一个比较各种贝叶斯和非贝叶斯模型检查和扩展方法的例子。  
<br/>
在 Draper (1995) 和 O’Hagan (1995) 的文章以及随附的讨论中，出现了关于模型选择和平均的各种观点。 我们建议读者阅读这些文章及其参考资料，以进一步讨论这些方法并提供示例。 因为我们强调模型的连续族而不是离散的选择，所以贝叶斯因子在我们的贝叶斯统计方法中很少相关。 参见 Raftery (1995) 和 Gelman and Rubin (1995) 关于这一点的两种截然不同的观点。  
<br/>
本章的最后一节是对 Rubin (1983a) 的详细阐述。 