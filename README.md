# Transfer-learning-for-CEUS
探讨自然场景下数据训练的模型能否较好迁移到数据量有限的超声造影视频中
本次研究使用公开发表的模型和参数，检索依据来自Paper With Code网站上计算机视觉-> 视频->视频分类任务的\href{https://paperswithcode.com/task/video-classification}{排名表}，部分公开模型虽然有着较好的性能，但因为作者认为这些网络的计算成本太高，如facebook开源的视频网络模型库\href{https://github.com/facebookresearch/VMZ}{VMZ}中的ir-CSN-152和ir-CSN-152模型，输入32\times $224^2$, 152层深度，没有纳入实验中。

本研究对比了2D CNN，2D CNN+BERT，C3D，I3D，SlowFast，R(2+1)D，TSM，GSM后，得到结论如下：
\begin{enumerate}
	\item GSM模型的性能较为优越；
	\item 2D CNN可以比部分时空网络效果好；
	\item BERT结构相对2D CNN，性能更好，且BERT resnet50 和BERT bninception性能较突出；
	\item Slow通道的迁移效果最差，不适合应用在CEUS中；
	\item TSM网络的迁移性能较差，这可能因为Shift模块的模式在CEUS上无法迁移利用；
	\item I3D的表现较为平庸；
	\item C3D对在更多数据时，性能提升明显，实现了0.9的正确率和0.867的ICC敏感性，在所有实验中变现最好的；
	\item R(2+1)D resnet34相比resnet34+BERT，性能提升更明显,基于resnet 50比resnet 34性能好，我们推断如果使用更深的resnet网络，性能会提升更高，因而是最有潜力的模型；
	\item 提高网络的视频采样的图像数目能够普遍地提升分类性能；
	\item 虽然原模型的训练基于密集采样，但均匀采样的迁移效果更好，因为数据包含了完整的造影剂增强和消退过程；
	\item 数据清洗虽然去除了21\%的图像，但平衡了HCC和ICC的数据量，直接使用原始数据时，有3/4的模型对ICC的敏感性变差，但在C3D上性能和敏感度都得到了提升，所以建议在具体使用对处理过和未处理数据集都测试；
	\item non-local结构虽然可以提升原问题的分类能力，但全局特征的关联不适合迁移到CEUS中；
	\item 在训练过程中，2D CNN的时间和计算消耗最少，训练是，1000次迭代在5分钟左右；2D+BERT稍微慢一点，但比GSM和TSM快；R(2+1)D的计算量较大；运行较慢；C3D，SlowFast, Slow基于3D卷积，计算量最大，运行时间最长，1000次迭代需要20分钟左右。
	\item 对不同网络对所有病例分类结果可视化可以发现，个别病例在绝大部分网络中都无法正确识别，这种局限可能来自数据集，也可能是迁移学习无法获得区分力强的特征。
\end{enumerate}

本研究是第一个系统研究视频模型在医学三维数据集中迁移能力的报告。
