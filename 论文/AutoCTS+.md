1 引言
在网络物理系统中,如交通系统、电力系统和污水处理系统等社会基础设施中,往往包含多个传感器,每个传感器都会发出一个时间序列,从而产生多个常常相关的时间序列。例如,在车辆交通系统中,感应线圈检测器会测量不同路段的时变交通流量,并且沿同一条路或附近路段的测量值通常是相关的。对这些相关时间序列进行未来值预测,往往具有重要应用。例如,准确预测交通流量可以有助于拥堵预测和近期出行时间预测,进而实现更有效的车辆路由规划等。

成功预测相关时间序列的关键是能够捕捉每个时间序列中历史值之间的时间依赖性,以及不同时间序列之间的空间相关性。利用深度学习模型强大的特征提取能力,提出了不同的神经网络架构(称为ST-Block)来捕获时空依赖性,从而实现准确预测。传统上,人工专家会手动设计ST-Block并选择相应的超参数设置。然而,这是一项资源密集型工作,不太适合人工专家。

最新的方法是自动设计高效的ST-Block。虽然现有的自动相关时间序列预测方法实现了设计自动化,并能够取得比人工设计的模型更好的性能,但它们仍然存在三个主要限制:

(1)缺乏支持联合搜索架构和超参数。现有方法在训练超网络时依赖于预定义的超参数,包括架构超参数(如ST-Block中潜在表示的数量和潜在表示的大小)和训练超参数(如dropout率)。根据超参数设置的不同,相同的架构可能会产生明显不同的性能。尽管如此,现有解决方案仍然依赖专家选择合适的超参数设置,这可能导致选择次优架构,使框架"半自动化"。或者,可以将超参数优化方法(如网格搜索或贝叶斯优化)与现有的自动架构搜索方法进行有序组合,以实现两步自动化方法。但是,由于每次采样一组超参数时,都需要执行现有的自动架构搜索,因此运行时间会过长。相反,我们需要一种高效的联合架构和超参数搜索方案。
(2)可扩展性差。现有的自动相关时间序列预测方法通常具有较差的可扩展性,因为在训练期间,整个超网络必须驻留在内存中,这在大规模相关时间序列设置中可能导致内存溢出。具体来说,组成超网络的神经操作员的内存成本会随着时间序列的数量N和时间序列中历史时间戳的数量P的增加而迅速增加。例如,常用的一维卷积神经网络(1D CNN,T-operator)和图卷积网络(GCN,S-operator)的内存成本会线性增加,而Transformer(S/T-operator)的内存成本会与P和N的平方成比例增加。由于超网络中每对节点之间有O个候选算子,因此超网络的内存成本大约是子网(即实际的CTS预测模型)的O倍。以图1(a)为例,超网络中每对节点之间有三种候选算子,即1D CNN、GCN和Transformer(用不同颜色线条表示)。因此,超网络的内存成本约为其子网的三倍。当增加N或P时,超网络的内存使用量会比子网更快达到上限,从而限制了神经架构搜索的可扩展性。

(3)一次性使用。现有的自动CTS预测方法针对每个特定数据集从头开始训练超网络,这是昂贵的。然而,考虑到不同CTS预测数据集之间可能存在相似之处,重用来自之前针对相关数据集的搜索所学到的知识可能会大大减少新数据集上的搜索时间。因此,开发可转移的自动化框架有望提高搜索效率。

我们提出了AutoCTS+框架,其核心是一种可扩展且高效的联合架构和超参数(SEARCH)策略,来解决上述限制。首先,我们设计了一个联合搜索空间,包含各种候选的[架构,超参数]对,每一对称为arch-hyper。然后,我们提出了一种新的搜索框架,利用架构-超参数比较器(AHC)来预测所有arch-hyper在联合搜索空间中的精度排名。AHC是一个神经网络,它接收两个候选arch-hyper的编码作为输入,并产生一个二进制标签,指示哪个输入arch-hyper的精度更高。为此,训练AHC需要大量形式为(ah1, ah2, y)的标记样本,其中ah1和ah2是arch-hyper,y是一个二进制标签。直观地,获取标签需要完全训练两个输入arch-hyper,这在计算上是昂贵的。为了降低成本,我们提出了一种易于获取的代理度量(proxy metric),用于生成伪标签,并以噪声减少的方式进一步训练AHC,以减少伪标签的负面影响。一旦获得arch-hyper排名,我们选择排名前K的arch-hyper进行完整训练,并最终选择验证精度最高的arch-hyper作为最终模型。此外,我们提出了一种简单而有效的迁移方法,避免在每个新数据集上从头开始学习AHC,该方法通过将在一个数据集上训练的AHC迁移到新数据集来实现。这样,只需要从新数据集获取少量AHC训练样本就可以对预训练的AHC进行微调,从而大大提高了搜索效率,且与在新数据集上从头开始训练AHC相比,精度并没有损失。

据我们所知,这是首次为相关时间序列预测启用联合搜索架构和相应超参数设置。具体来说,我们做出了以下贡献:

(1) 我们提出了一个新的搜索空间,用于相关时间序列预测,以促进联合搜索架构和超参数设置。

(2) 提出了一种内存高效的架构-超参数比较器(AHC),用于对arch-hyper候选进行排名,它包含一个易于获取的代理度量来生成伪标签,以去噪声的方式训练AHC,从而提高搜索效率。

(3) 我们提出了一种能够快速将训练好的AHC适应到新数据集的方法,从而大幅提高了在新数据集上训练AHC的效率。

(4) 在六个基准数据集上进行的大量实验表明,AutoCTS+能够比现有手动和自动方法更高效地找到性能更好的CTS预测模型。

2 preliminaries

2.1 问题设置
相关时序列(CTS)。我们用X∈RN×T×F表示N个相关时间序列(CTS),其中每个时间序列包含T个时间戳,在每个时间戳处有一个F维特征向量。X中第i个时间序列X(i)∈RT×F⊂X(1≤i≤N)中的特征向量不仅与该时间序列中先前的特征向量相关,还与其他时间序列中的特征向量相关。因此,很自然地将CTS建模为一个图G=(V, E, A),其中顶点集V表示时间序列集合,边集E表示时间序列之间的相关关系,邻接矩阵A∈RN×N捕获了时间序列之间关系的强度。通常A是基于生成时间序列的传感器之间的距离预先定义的,或者是自适应学习的。

相关时间序列预测。我们考虑多步和单步相关时间序列预测,这两种都有重要的现实应用。给定X的过去P个时间步的特征向量,多步CTS预测的目标是预测未来Q(Q>1)个时间步的特征向量;而单步CTS预测的目标是预测第Q(Q≥1)个未来时间步的向量。形式上,我们定义多步CTS预测如下:

(X̂t+P+1, X̂t+P+2, ..., X̂t+P+Q) = F(Xt+1, Xt+2, ..., Xt+P; G)

而单步CTS预测定义如下:

X̂t+P+Q = F(Xt+1, Xt+2, ..., Xt+P; G)

其中Xt∈RN×F表示所有时间序列在时间戳t处的特征向量;而X̂表示预测的特征向量,F是CTS预测模型。

问题定义。目标是从预定义的联合架构-超参数搜索空间S中自动构建一个最优的ST-Block F*,使其在验证数据集Dval上的预测误差最小。数学上,目标函数可以表述为:

F* = argmin(F∈S) ErrorMetric(F, Dval)

3 相关工作

3.1 传统相关时间序列预测方法
早期的相关时间序列预测方法大多基于经典的时间序列模型,如自回归模型(AR)、向量自回归模型(VAR)以及其扩展版本。这些方法主要捕捉了时间序列中的时间依赖关系,但无法很好地捕获时间序列之间的空间相关性。

为了解决这一缺陷,一些考虑空间相关性的模型被提出,如空间时间自回归模型(STAR)和高斯马尔可夫随机场(GMRF)。STAR通过矩阵向量算子捕获空间相关性,而GMRF则利用图模型来捕获相关时间序列之间的依赖关系。尽管这些传统模型展现了一定的性能,但它们在处理大规模复杂数据时存在局限性,也难以捕捉复杂的时空模式。

3.2 基于深度学习的相关时间序列预测
近年来,基于深度学习的方法在相关时间序列预测任务中取得了卓越表现,展现出强大的时空特征提取能力。一些研究直接将标准神经网络(如RNN、CNN等)应用于展开后的时间序列输入。但由于没有明确考虑时间序列之间的相关性,因此性能受到限制。

为了更好地捕捉时空依赖关系,专门设计了各种ST-Block神经网络架构。这些ST-Block通常包含两种主要类型的模块:时间(Temporal)模块和空间(Spatial)模块。时间模块通常由卷积层或RNN捕捉单个时间序列中的时间依赖关系;而空间模块则利用图卷积网络(GCN)或注意力机制来捕获不同时间序列之间的空间相关性。

这些手工设计的ST-Block取得了一定的成功,但仍存在一些固有缺陷。首先,它们依赖于领域专家的经验来设计合适的ST-Block架构,这种设计过程是主观的且有偏差的。其次,对于不同的数据集,最优架构可能不同,而手动调整架构和超参数非常耗时且需要大量尝试。

3.3 自动神经架构搜索
为克服手工设计模型的缺陷,自动神经架构搜索(NAS)越来越受到重视。NAS通过在预定义的搜索空间中搜索最优模型架构,实现了自动设计神经网络的目标。现有的NAS方法可大致分为三类:基于强化学习(RL)的、基于进化算法(EA)的和基于梯度的NAS。

在RL-based NAS中,RL代理生成新架构并根据其在验证集上的表现来学习更新策略。EA-based NAS借鉴了生物进化,通过对初始种群中的架构进行变异和选择来进化出性能更佳的架构。Gradient-based NAS最近备受关注,它将NAS问题重新形式化为某种连续优化形式,从而可以通过梯度下降有效解决。其中一种广为采用的方法是基于超网的NAS,它将所有候选架构统一编码到称为超网的复杂网络中,然后通过训练超网,根据各算子的架构权重获得最优子网架构。

虽然这些NAS方法已经在计算机视觉、自然语言处理等领域取得了成功,但将它们直接应用于高维序列预测任务(如相关时间序列预测)仍然存在许多挑战。首先,大多数现有NAS方法专注于搜索低层表示,而非高层端到端预测模型,这限制了其在序列预测任务中的应用。此外,现有NAS方法通常无法处理图结构化输入,而相关时间序列数据本质上是图结构化的。最后,现有NAS框架还无法同时搜索最佳架构和超参数,而只是在固定超参数下搜索最优架构。

3.4 自动相关时间序列预测
为解决上述挑战,一些专门的自动CTS预测框架最近被提出。例如,AutoSTGCN通过进化策略同时搜索时间和空间卷积核;而AutoST则采用基于梯度的NAS来搜索各种候选ST-Block。这些自动化方法已显示出优于人工设计的性能。然而,它们存在一些共同的重大缺陷:1)无法联合搜索最优架构及其对应的超参数配置;2)基于超网络的搜索空间受到内存限制,难以扩展到大规模数据;3)针对每个新数据集,都需要从头开始昂贵的搜索过程。本文中提出的AutoCTS+旨在解决这些关键性缺陷。

4 THE AUTOCTS+ FRAMEWORK

为解决现有自动CTS预测方法的局限性,我们提出了AutoCTS+框架。如图1(b)所示,AutoCTS+由三个主要组件组成:

(1) 联合搜索空间:一个统一的编码方案,用于表示各种候选的[架构,超参数]对,称为arch-hyper。

(2) 架构-超参数比较器(AHC):一个神经网络,用于预测任意两个arch-hyper之间的精度排名关系。

(3) 自适应微调策略:一种迁移学习策略,可借助之前在相关数据集上训练的AHC,快速将其适应到新数据集上。

下面将详细介绍这三个关键组件。

4.1 联合搜索空间
传统的NAS框架通常将架构搜索与超参数选择分开进行。一些最新工作虽然尝试将它们统一到超网络中,但由于计算和内存开销过大而难以扩展。相比之下,我们的联合搜索空间是一种更简单、更高效的编码方案,能紧凑地表示各种[架构,超参数]对。

具体来说,一个arch-hyper由以下几个部分组成:(1)一个ST-Block架构编码,表示ST-Block中各算子类型及它们之间的连接方式;(2)一个架构超参数向量,指定了诸如ST-Block中节点数量、隐藏单元大小等设置;以及(3)一个训练超参数向量,指定了诸如学习率、dropout率等设置。

ST-Block架构编码。我们使用一种基于序列的编码来表示ST-Block架构,灵感来自于现有工作。具体来说,对于包含L个节点(latent representations)的ST-Block,我们使用长度为2L-1的序列来编码其架构,每个元素取值于{0,1,...,K}。其中K是可选算子类型的总数,包括K-1种标准算子类型(如卷积、注意力等)和一种None算子(表示无算子)。在这个长度为2L-1的编码序列中,第一个L个元素分别表示从输入节点到第i个节点的算子类型,而剩余的L-1个元素则表示从第(i+1)个节点到输出节点的算子类型。例如,考虑一个包含3个节点{h1,h2,h3}的ST-Block,如果编码序列为[1,2,0,1,3],则表示:从输入到h1使用算子1,从输入到h2使用算子2,从输入到h3没有算子(0表示None),从h2到输出使用算子1,从h3到输出使用算子3。

对于传统NAS方法来说,这种扁平序列编码无法表示复合算子(如残差连接、跳跃连接等)。但对于CTS预测来说,复杂连接往往并非必需。因此,为了简洁起见,本文中的ST-Block架构只包含基本算子,而不支持复合算子。不过,这种编码方式可以很容易地扩展以支持更复杂的连接模式。

超参数编码。我们将与架构相关的超参数(如节点数量、隐藏单元大小等)和与训练相关的超参数(如学习率、dropout率等)分别编码为向量。然后,将ST-Block架构编码与这两个超参数向量连接,即得到完整的arch-hyper编码。

4.2 架构-超参数比较器(AHC)

训练AHC是AutoCTS+框架的核心部分。直观地说,AHC需要学习预测任意两个输入arch-hyper之间的精度排名关系。具体来说,给定两个arch-hyper的编码(ah1,ah2),AHC将输出一个二进制标签y,指示ah1的精度是否高于ah2。因此,训练AHC需要大量形如(ah1, ah2, y)的标记样本对。

获取标记样本的一种天然方式是先完全训练输入的两个arch-hyper(ah1和ah2),然后根据它们在验证集上的表现(即精度)来确定标签y。不过,这种方式代价高昂,因为需要对每个arch-hyper都进行完整的训练过程。为了降低标记成本,我们提出了一种易于获取的代理度量(proxy metric),用于为arch-hyper对生成伪标签,然后在这些伪标签的指导下预训练AHC。

代理度量。我们的关键观察是:精度更高的arch-hyper往往能以更快的速度达到较低的训练损失。因此,我们定义了基于训练损失的代理度量,用作生成伪标签的依据。具体来说,对于arch-hyper对(ah1, ah2),首先在验证集Dval上计算它们的早期训练损失(如训练轮次k=5时的损失),记为valk1和valk2。我们将具有更小早期训练损失的arch-hyper视为暂时"更好",并据此生成伪标签:

y = 1, if valk1 < valk2
0, otherwise

这种基于代理度量的标记方式速度很快,只需训练几个epoch即可得到损失值。虽然所得伪标签可能有噪声,但我们之后将采取去噪措施来减轻其影响。

预训练AHC。利用上述方法,我们可以获得大量(ah1, ah2, y)形式的训练样本对,用于预训练AHC。具体来说,将arch-hyper对(ah1, ah2)的编码串联作为AHC的输入,AHC会输出一个二进制预测概率值p,与真实标签y进行交叉熵损失。通过在大量样本对上最小化损失,AHC就能逐步学习预测输入arch-hyper对的精度排名关系。

在预训练过程中,由于是基于噪声伪标签进行训练,因此AHC的预测结果可能存在一些偏差。为此,我们进一步采用一种同态嵌入一致性正则化方法,以减小AHC的判断误差。具体来说,将arch-hyper编码输入到AHC后,在输出logits之前,添加一个紧凑的同态嵌入层,将logits映射到一个较低维的嵌入空间。我们鼓励相似arch-hyper具有相近的嵌入,而不同arch-hyper具有不同的嵌入,从而实现一种正则化效果,提高嵌入的判别性。

总之,预训练阶段的损失函数包括两部分:交叉熵损失和嵌入一致性正则化损失。通过端到端训练,AHC能够较为准确地学习arch-hyper对的排名关系。

4.3 自适应微调策略

预训练的AHC虽然能够较准确地对arch-hyper对进行排名,但其性能仍然会受到噪声标签的影响。为进一步提升AHC的排名准确性,我们采用一种自适应微调策略,利用少量真实标记样本对预训练模型进行微调。

具体来说,对于新的CTS预测数据集,我们首先从联合搜索空间S中采样一组候选arch-hyper对{(ah1, ah2)}。然后,我们对每个arch-hyper对执行少量(如5)epochs的完整训练,并根据它们在验证集上的实际表现(即精度)来获取真实标签y。以这种方式,我们可以获得一小批(ah1, ah2, y)形式的真实标记样本。

利用这些真实标记样本,我们对预训练的AHC进行少量epoch(如10个)的微调训练。在微调过程中,我们将AHC视为一个特征提取器,在其输出logits之前追加一个新的分类头,并仅微调该分类头的参数。通过这种"自适应"微调,AHC不仅能保留之前从大量噪声标签中学习到的一般知识,还能进一步利用少量真实标签在新数据集上"适应"并提高其排名准确性。

4.4 搜索最终CTS预测模型
利用经过微调的AHC,我们即可对候选arch-hyper对进行高效排名,从而识别出最优arch-hyper对。具体来说:

1. 从搜索空间S中采样M个arch-hyper;
2. 利用AHC对所有M(M-1)/2个arch-hyper对进行精确排序;
3. 选择排名前K个arch-hyper;
4. 对前K个arch-hyper进行完整训练,并选择在验证集上表现最佳的作为最终CTS预测模型。

通过这种两阶段优化策略,AutoCTS+能够在大范围内快速识别出优质arch-hyper,从而避免了对所有候选arch-hyper进行全量训练,大大提高了整体搜索效率。

5 实验
5.1 实验设置
数据集。我们在6个公开基准数据集上评估AutoCTS+,包括4个交通数据集(METR-LA、PEMS-BAY、PEMSD7M和PEMSD8M)和2个其他领域数据集(SRID和d-ECG)。数据集的规模从数十个传感器到数百个传感器不等。

评估指标。我们采用平均绝对误差(MAE)和平均绝对百分比误差(AMAPE)作为评估指标,这两个指标广泛用于相关时间序列预测任务。5.2 搜索空间详情

我们的联合搜索空间包含了广泛的ST-Block架构和对应的超参数配置选择。

ST-Block架构选择。我们考虑了4种常用的时间算子:causality convolution (Cconv)、dilated causality convolution (Dilconv)、gated recurrent unit (GRU)和long-short term memory (LSTM)。对于空间算子,我们使用了图卷积网络(GCN)、图注意力网络(GAT)和基于Transformer编码器层的图Transformer(GTR)。此外,我们还支持无操作(None),以允许架构中存在恒等映射。因此,总共有9种可选算子类型({Cconv, Dilconv, GRU, LSTM, GCN, GAT, GTR, None})用于构建ST-Block架构。

架构超参数。我们考虑了以下与ST-Block架构相关的超参数:模型通道数(隐藏单元)、ST-Block中节点数量L、以及各种算子的卷积核大小。

训练超参数。我们包括以下常用训练超参数:学习率、优化器(Adam或SGD)、正则化(L1/L2正则、dropout)等。

总之,我们的联合搜索空间包含了广泛的ST-Block架构选择(由9种算子类型构成)以及60多个与架构和训练相关的超参数,从而为探索高性能CTS预测模型提供了足够的自由度。

5.3 implementation details
AHC细节。我们的AHC是一个简单的前馈神经网络,具有4个隐藏层,每层包含128个单元。每个arch-hyper编码作为输入,首先通过一个嵌入层映射为128维稠密向量,然后依次通过4个隐藏层。之后是一个256维的同态嵌入层,最后是一个二分类头输出logits。预训练AHC时,我们使用Adam优化器,学习率为1e-4,批量大小为64。

代理度量细节。我们在每个数据集上选择了最佳的训练轮次k,用于计算arch-hyper的早期训练损失。具体来说,选择k=5用于METR-LA、PEMSD7M和SRID,k=3用于PEMS-BAY和PEMSD8M,以及k=10用于较小的D-ECG数据集。

搜索细节。为充分利用AHC的预测能力并加速收敛,我们采用蒙特卡洛树搜索的顶层启发式方法进行arch-hyper采样。具体来说,搜索空间被抽象为一个kd树,其中每个节点表示一个部分编码的arch-hyper。我们从树根出发,利用AHC逐层选择高分支继续拓展,直到生成一个完整的arch-hyper编码。为确保质量和多样性,我们每隔25个arch-hyper即重新初始化采样过程。对于每个数据集,我们总共采样了500个arch-hyper,选取其中精度最高的前5%进行完整训练。

5.4 实验结果
表1总结了AutoCTS+以及其他自动和手工设计的基线方法在六个数据集上的表现。我们可以观察到以下几点:

(1) 与NAS-CTS类的自动方法相比,AutoCTS+显示出明显的性能优势,在所有数据集上的平均MAE和AMAPE误差均最低。这验证了AutoCTS+联合搜索架构和超参数的优越性。

(2) 与披萨类的专家设计模型相比,AutoCTS+也展现出更好或相当的性能。这突显了AutoCTS+自动发现优质CTS预测模型的能力。

(3) 相比所有传统的人工设计模型(ARIMA、VAR等),AutoCTS+获得了更大的性能提升。

(4) AutoCTS+在各种规模大小的数据集上都表现良好,体现了其优异的泛化能力。

总之,AutoCTS+展示了令人信服的优异性能,验证了其作为一种自动化解决方案用于相关时间序列预测任务的有效性。