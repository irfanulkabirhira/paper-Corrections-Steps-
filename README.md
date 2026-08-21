i have submitted my paper on "Discover Artificial Intelligence "
Paper Title: "Privacy Preserving and Explainable Federated Learning for Brain Tumor MRI Classification Using LiteGAN FedNet"

and i have attached the complete Overleaf file of my paper :  at fisrtv read my entire paper from the overleaf, and then you will help me out to fix the corrections below corrected after : 
---------------------------------------------------------------------------------------------------

Reviewer comments


Reviewer 1

This is a very good, well-executed, and timely manuscript that makes a clear and valuable contribution. Having addressed the suggestions below, this paper could be a strong accept.

The manuscript presents LiteGAN-FedNet, a highly effective and communication-efficient federated learning pipeline designed for multi-class brain tumor MRI classification. The core contribution is a client-side optimization sequence that performs conditional GAN-based data augmentation directly within the latent feature space of a ResNet-18 backbone, paired with PCA compression to reduce the transmitted parameter volume by 80.5 percent. The integration of a multi-perspective explainability framework combining Grad-CAM, SHAP, and LIME, along with robust empirical performance on the Figshare and Mendeley benchmarks, represents a strong and clinically valuable advance.

1. The authors should add a paragraph in Section 3.5 clarifying the specific layers of the ResNet-18 backbone that were fine-tuned versus those kept frozen, and discuss how the pre-trained ImageNet weights are robust enough to capture distinct tumor morphology before the cGAN training begins.

2. Part A. In Section 3.7 and Algorithm 1, the manuscript states that each client independently fits PCA on their local augmented features to compute a client-specific projection matrix.
Part B. Because PCA is highly data-dependent, the local principal components (coordinate axes) generated on different client datasets will represent entirely different orthogonal bases, meaning that training local MLPs on these disparate feature spaces and aggregating their parameters via standard FedAvg is mathematically inconsistent.
Part C. To resolve this concern, the authors should add a detailed explanatory paragraph in Section 3.7 clarifying whether a global projection matrix was computed and distributed, or explicitly discuss how the framework handles coordinate misalignment during aggregation.

3. The authors should add a brief explanatory paragraph in Section 4.7 explaining how the quality and diversity of the cGAN-generated latent features were verified, as traditional image-based metrics like FID cannot be applied directly to 512-dimensional vector spaces, and discuss if any feature-space collapse was observed during local client training.

4. Some relevant citations are included but several highly relevant are missing: A. Data Storage, Cloud Usage and Artificial Intelligence Pipeline. Artificial Intelligence in Cardiothoracic Imaging (2022). B. Foundation AI Model for Medical Image Segmentation. arXiv preprint (2024). C. ChatGPT in medical publications. Radiology (2023). D. A survey for large language models in biomedicine. arXiv preprint (2024). E. Predictable LLM Serving on GPU Clusters. arXiv preprint (2025). F. The Trust Fabric: Decentralized Interoperability and Economic Coordination for the Agentic Web. arXiv preprint (2025). G. The hidden adversarial vulnerabilities of medical federated learning. arXiv preprint (2023). H. Structured Robustness for Distribution Shifts. ICLR (2025). I. GPU Tail Latency Diagnosis for Serverless and HPC Workloads using eBPF. Proceedings of the 11th International Workshop on Serverless Computing (2025). J. Elastic MIG Reconfiguration with PCIe-Aware Placement for Multi-Tenant GPUs. Proceedings of the 11th International Workshop on Serverless Computing (2025). K. Scaling Test-Time Compute Can Outperform Larger Architectures in Computer Vision. CVPR (2025). L. Host-Side Telemetry for Performance Diagnosis in Cloud and HPC GPU Infrastructure. arXiv preprint (2025). M. Weight-space noise for privacy-robustness trade-offs in federated learning. Neural Computing and Applications (2025). N. Fed-Safe: Securing federated learning in healthcare against adversarial attacks. arXiv preprint (2023)


5. Part A. The multi-level explainability framework combines Grad-CAM to visualize ResNet-18 features, LIME for raw image perturbations, and SHAP to attribute importance to the PCA-compressed 100-dimensional components fed to the MLP classifier.
Part B. Because these three explainability techniques operate on entirely different representational domains (spatial pixels, deep convolutional channels, and abstract linear projection components), there is an unaddressed representational mismatch that makes it conceptually difficult to verify if they provide consistent, non-contradictory clinical evidence.
Part C. The authors should add a paragraph in Section 5 discussing this representational divergence among the XAI methods, and clarify how a clinician can practically map a high-attribution SHAP principal component back to the anatomical visual explanations provided by LIME and Grad-CAM.


Reviewer 2

Comment #1: Literature Review
The novelty claim is currently broader than what the literature review establishes. The manuscript states that LiteGAN-FedNet differs from existing FL+GAN frameworks through client-side feature-space augmentation, PCA compression, and explainability, but the related work mainly reviews centralized GAN-based synthesis, general MRI classification, and some neuroimaging studies. The review does not sufficiently analyze prior federated medical imaging works that address class imbalance, communication reduction, privacy protection, feature-level augmentation, or federated GAN-based learning. As a result, the reader is not given enough evidence to understand which exact gap the proposed method fills.
Suggested revision: The literature review should be reorganized around the claimed contributions: federated brain tumor classification, GAN-based augmentation in FL, communication-efficient FL/compression methods, privacy mechanisms in FL, and XAI for MRI diagnosis. For each group, the authors should explain what existing methods do, what they do not address, and how LiteGAN-FedNet differs. In particular, any methods listed later as competitors, especially GAN- or FL-related approaches, should be discussed in the related work rather than appearing only in the comparison table.

Comment #2: Methodology
The PCA-based federated learning mechanism is not methodologically clear. The manuscript states that each client fits PCA locally on its augmented feature set, but if each client learns its own PCA basis, the resulting 100-dimensional representations are not guaranteed to share the same coordinate system. Aggregating MLP weights through FedAvg over client-specific PCA spaces can be invalid unless the PCA basis is shared or aligned. In addition, the manuscript states that PCA reduces transmitted feature volume or parameter volume, but FedAvg usually transmits model updates, not feature embeddings. It is therefore unclear what exactly is being communicated and how the 80.5% reduction is calculated.
Suggested revision: The authors should explicitly define whether PCA is local, global, or shared across clients. If PCA is local, they must explain how PCA component ordering, signs, and coordinate systems are aligned before FedAvg aggregation. If a shared PCA is used, they should explain how it is obtained without violating privacy, for example through secure aggregation of covariance statistics or a training-only public/shared basis. The communication analysis should be rewritten in terms of actual transmitted quantities, such as number of parameters, bytes per round, total communication over 50 rounds, and whether features or model updates are transmitted.

Comment #3: Methodology
The cGAN component is central to the proposed contribution, but its training procedure is insufficiently specified and partly inconsistent. Section 3.6 describes local cGAN training, Table 2 only reports latent dimension and synthetic embeddings per class, and Algorithm 1 appears to train the cGAN once before federated training. However, the text later states that LiteGAN-FedNet embeds cGAN augmentation within each federated round. These alternatives have different computational costs and different effects on convergence. The discriminator/generator layer widths, activation details, training epochs, gradient penalty coefficient, discriminator-to-generator update ratio, stopping criteria, and class-balancing rule are not reported.
Suggested revision: The authors should provide a complete cGAN specification, including generator/discriminator architectures, hidden dimensions, optimizer settings, WGAN-GP coefficient, number of cGAN epochs, update ratio, latent sampling strategy, and exact rule for generating synthetic samples per class. They should also state whether the cGAN is trained once before FL, periodically, or in every communication round. If synthetic features are generated every round, the added computation should be measured. If they are generated once, the text and algorithm should be corrected accordingly.

Comment #4: Methodology
The privacy-preserving claim is not fully supported by the described methodology. The manuscript states that secure aggregation is used and differential privacy is optional, but it does not define a threat model, does not report an implemented privacy budget, and does not provide clipping norms, noise multipliers, ε, δ, or utility trade-offs. Also, feature embeddings and synthetic feature generation can still carry privacy risks, including reconstruction, membership inference, or memorization by the local generator. Simply avoiding raw image sharing is not sufficient to claim strong privacy preservation.
Suggested revision: The authors should separate what is actually implemented from what is only optional. If differential privacy is implemented, they should report the privacy accountant, ε/δ values, clipping strategy, noise scale, and performance under DP. If DP is not implemented, it should not be used to support the main privacy claim. The authors should also define the assumed adversary and discuss privacy risks specific to feature embeddings and GAN-generated representations. At minimum, the manuscript should revise the language from “privacy-preserving” to “raw-data-local federated learning” unless formal privacy protection is demonstrated.

Comment #5: Experiments
The dataset description is too limited for reproducibility and may contain ambiguity regarding modality and class composition. The manuscript states that both datasets contain T1 and T2 weighted axial MRI images and four classes, including no-tumor, but the exact source structure, number of images, patient/sample counts, class distribution, slice-level versus patient-level organization, and modality composition are not clearly reported. Without this information, it is difficult to assess whether the splits are clinically meaningful or whether the reported performance may be affected by dataset overlap, duplicate slices, or overly simple public benchmark characteristics.
Suggested revision: The authors should provide a detailed dataset description for each dataset, including total images, number of patients if available, number of samples per class, imaging modality, acquisition characteristics, and whether the data are slice-level or patient-level. The train/test split should be described at the patient level whenever patient identifiers are available. If only image-level splitting is possible, the authors should explicitly acknowledge the risk of leakage or near-duplicate slices and provide a de-duplication or similarity-check procedure.

Comment #6: Experiments
The non-IID federated setup is under-specified and repeated in several places without quantitative definition. The manuscript says that client distributions are intentionally non-IID or weakly non-IID, but it does not provide the class counts per client, the sampling method, the heterogeneity parameter, or whether the same client partition is used across random seeds. Since the paper’s motivation depends heavily on non-IID class imbalance, this missing detail prevents the reader from judging whether the experimental setting is realistic or sufficiently challenging.
Suggested revision: The authors should report the exact class distribution for each of the five clients for both datasets. They should state whether the partition was created using Dirichlet sampling, fixed class-skew rules, quantity skew, label skew, or another method. The authors should also evaluate at least two levels of heterogeneity, such as IID, mild non-IID, and strong non-IID, to show whether LiteGAN-FedNet remains effective when client drift becomes more severe.

Comment #7: Experiments
The implementation details are not sufficient for reproduction. The manuscript mentions Python, PyTorch/TensorFlow, Flower, ImageDataGenerator, GPU RTX 3090, one random seed, and multiple random seeds, but it does not provide exact library versions, seed values, training times, code link, early stopping rules, learning-rate schedules, ResNet fine-tuning details, or computational cost of local cGAN training. The source code statement says “GitHub Repository” but does not provide an actual repository link.
Suggested revision: The authors should provide a reproducibility checklist or appendix with exact software versions, hardware configuration, random seeds, data split files or split-generation scripts, optimizer settings for every module, ResNet fine-tuning schedule, MLP architecture details, cGAN training settings, and runtime/memory requirements per client. The GitHub repository should be provided as a real link or removed until available. The manuscript should also resolve the inconsistency between “same random seed” and “multiple random seeds.”

Comment #8: Results
The interpretability and statistical analyses are not rigorous enough to support the strength of the claims. The Grad-CAM and LIME figures are interpreted as focusing on tumor-relevant regions, but no radiologist review, tumor masks, localization metric, or failure-case analysis is provided. SHAP is applied to PCA components, but the clinical meaning of individual principal components is not directly interpretable without mapping them back to image or feature patterns. For McNemar’s test, the manuscript reports only p-value thresholds and does not provide the paired disagreement counts, exact p-values, baseline predictions, or correction for multiple comparisons.
Suggested revision: The authors should add quantitative or expert-supported XAI validation. For example, if masks or approximate tumor regions are available, report localization overlap between Grad-CAM/LIME and tumor areas; otherwise, include blinded radiologist assessment or at least representative failure cases where explanations are misleading. For SHAP, the authors should avoid claiming direct clinical meaning for PCA components unless they provide a mapping or analysis connecting those components to interpretable image features. For McNemar’s test, they should report the exact paired contingency tables, exact p-values, baseline method details, and whether the test was performed separately for each dataset and each seed.''


Comment #9: Results
The ablation study does not fully isolate the contribution of each component. The “Full LiteGAN-FedNet” result improves substantially over “FL + PCA + cGAN,” but the stated additional components include explainability modules, which should not change classification accuracy if they are post-hoc. This creates uncertainty about what actually caused the final increase. The ablation is also not reported separately for Figshare and Mendeley, and no standard deviations or confidence intervals are given for each ablated variant.
Suggested revision: The authors should restructure the ablation so that every performance change corresponds to a train-time component. Suggested variants include FL only, FL + PCA, FL + cGAN, FL + PCA + cGAN, FL + PCA + cGAN + secure aggregation, and FL + PCA + cGAN + DP if DP is actually implemented. XAI should be reported separately as interpretability analysis, not as a performance-improving component. Each ablation should be reported per dataset with mean ± standard deviation or confidence intervals over the same seeds.

Comment #10: Reproducibility
You have developed a new method. The authors should include a comprehensive appendix, regardless of whether the implementation code is released in other platforms like GitHub, etc. This appendix should present two clear tables: one summarizing all key experimental settings (e.g., hyperparameters, optimizer, learning rate schedule, epochs, batch size, weight decay, dropout, and other relevant training details), and another describing the hardware platforms used for experimentation, along with brief explanations. Such transparency improves reproducibility, facilitates future research, and helps advance the field.




"Up to all comments you will help me out to fix "

as well as i have provided you the complete coding section, which is  in ipynb file , then i will let you provide the entire Overleaf. First, at first undestand the coding section from screcth ...

And listen 
each of the correction provdided Correction proper, 
Must tell me where to place that in my overleaf , beacuse i won't be able to find them !!!
And also help me out to fix them in the correct place , as well as if anything need to delete , then also do that !!!
so that i find the exact place to correct !!! Start from comment  1 numeber then when it will be  you will let me know you are done with that number
then you will move forward .... sequentially we will solve them out ...

let me provide you how it should be : just for example to understand from another paper 
------------------------------
Demo one : 
============
Fix for Issue #1 & #2 — three separate edits in Overleaf
Edit A — Add a new subsubsection to Section III-F
Where: In your .tex, go to \subsection{Multimodal Consistency Fusion and Verification} (\label{sec:consistency_fusion}).
Find this exact text (it's the \subsubsection{Weighted Consistency Fusion} block — search for w_1=0.5):

Therefore, the final consistency score is calculated as:
\begin{equation} S_{\mathrm{EMCR}} = 0.5S_{TI} + 0.3S_{TO} + 0.2S_M. \end{equation}
Here $S_{TI}$ is given more importance, because semantic consistency between text and visual content plays a major role. On the other hand, OCR-based and metadata-based evidence provide additional contextual and structural support \cite{wu2025balanced}.
Since each component score lies in the interval $[0,1]$, the final consistency score also follows the same range:
\begin{equation} S_{\mathrm{EMCR}}\in[0,1]. \end{equation}
\subsubsection{Verification Decision Rule}

Insert a brand-new subsubsection right before \subsubsection{Verification Decision Rule} (i.e., paste this in between the S_{\mathrm{EMCR}}\in[0,1] equation block and \subsubsection{Verification Decision Rule}):

\subsubsection{Category Prediction via Learned Fusion Classifier}
In addition to the scalar consistency score $S_{\mathrm{EMCR}}$, EMCR trains a lightweight learned fusion classifier, $\mathrm{MLP}_{\phi}$, that predicts the six-way veracity category directly. Rather than operating on $S_{TI}, S_{TO}, S_M$, this classifier takes the class-probability vectors produced by the two supervised branches as input:
\begin{equation}
\hat{c} = \arg\max\; \mathrm{softmax}\big(\mathrm{MLP}_{\phi}([\mathbf{p}_{\mathrm{CLIP}};\,\mathbf{p}_{\mathrm{XGB}}])\big),
\end{equation}
where $\mathbf{p}_{\mathrm{CLIP}}\in\mathbb{R}^{6}$ and $\mathbf{p}_{\mathrm{XGB}}\in\mathbb{R}^{6}$ are the softmax class-probability outputs of the CLIP semantic branch and the XGBoost metadata branch, respectively, and $[\cdot;\cdot]$ denotes vector concatenation. $\mathrm{MLP}_{\phi}$ consists of a single hidden layer with ReLU activation and dropout, trained with cross-entropy loss against the six ground-truth categories.
The category prediction $\hat{c}$ is what is reported in the per-class precision/recall/F1 tables and confusion matrices throughout Section~\ref{sec:Results and Discussion}. It is a distinct output from the binary consistency decision $\hat{y}$ produced by the threshold rule described next: $\hat{c}$ answers \emph{which of the six categories the instance belongs to}, while $\hat{y}$ answers \emph{whether the available evidence is mutually consistent}, independent of category. Both outputs are returned together by the pipeline (Algorithm~\ref{alg:emcr}) and are reported alongside one another in the explainable reasoning output (Section~\ref{sec:explainability}).

Edit B — Update Algorithm 1
Where: Find \begin{algorithm}[htpb] (\label{alg:emcr}).
Find this exact block:

\Procedure{CrossModalConsistencyFusion}{}
    \State Construct consistency representation: $F_C = [S_{TI}, S_{TO}, S_M]$
    \State Generate final consistency score via weighted fusion:
    \[
    S_{\mathrm{EMCR}} = w_1 S_{TI} + w_2 S_{TO} + w_3 S_M
    \]
\EndProcedure
\medskip
\Procedure{ConsistencyBasedVerification}{}

Replace it with (this inserts a new procedure between the two existing ones):

\Procedure{CrossModalConsistencyFusion}{}
    \State Construct consistency representation: $F_C = [S_{TI}, S_{TO}, S_M]$
    \State Generate final consistency score via weighted fusion:
    \[
    S_{\mathrm{EMCR}} = w_1 S_{TI} + w_2 S_{TO} + w_3 S_M
    \]
\EndProcedure
\medskip
\Procedure{LearnedFusionClassification}{}
    \State Retrieve CLIP class probabilities $\mathbf{p}_{\mathrm{CLIP}}$ and XGBoost class probabilities $\mathbf{p}_{\mathrm{XGB}}$
    \State Concatenate: $\mathbf{v} = [\mathbf{p}_{\mathrm{CLIP}}; \mathbf{p}_{\mathrm{XGB}}]$
    \State Predict six-way category via learned MLP:
    \[
    \hat{c} = \arg\max\; \mathrm{softmax}(\mathrm{MLP}_{\phi}(\mathbf{v}))
    \]
\EndProcedure
\medskip
\Procedure{ConsistencyBasedVerification}{}

Then find this block a bit further down:
\State \textbf{Main Execution:}
\State \Call{SemanticConsistencyAnalysis}{}
\State \Call{OCRGuidedTextualConsistency}{}
\State \Call{MetadataStructuralReasoning}{}
\State \Call{CrossModalConsistencyFusion}{}
\State \Call{ConsistencyBasedVerification}{}
\State \Return Predicted class $\hat{y}$, consistency score $S_{\mathrm{EMCR}}$, explanation vector $E$

Replace with:
\State \textbf{Main Execution:}
\State \Call{SemanticConsistencyAnalysis}{}
\State \Call{OCRGuidedTextualConsistency}{}
\State \Call{MetadataStructuralReasoning}{}
\State \Call{CrossModalConsistencyFusion}{}
\State \Call{LearnedFusionClassification}{}
\State \Call{ConsistencyBasedVerification}{}
\State \Return Predicted category $\hat{c}$, consistency label $\hat{y}$, consistency score $S_{\mathrm{EMCR}}$, explanation vector $E$

Edit C — Clarify the two outputs in the Explainable Reasoning section
Where: \subsection{Explainable Reasoning Output} (\label{sec:explainability}).
Find this exact text (the opening paragraph):

To improve clarity and explainability, the proposed Explainable Multimodal Consistency Reasoning (EMCR) framework exposes each modality-specific evidence signal that contributes to the final verification result. Instead of providing only a prediction label, this framework provides intermediate consistency estimates derived from semantic alignment, OCR-guided textual agreement, and metadata-based structural evidence. Such intermediate evidence exposure has been shown to be helpful in enhancing explainability, reliability, and user trust in multimodal misinformation analysis \cite{lekshmiammal2025reasoning,thakur2026interpretable}.

Insert a new sentence immediately after it (before Let, / the \mathcal{E} equation that follows):

It is important to distinguish between the two outputs the framework returns for each instance: a six-way category prediction $\hat{c}$, produced by the learned fusion classifier described in Section~\ref{sec:consistency_fusion}, and a binary consistency label $\hat{y}\in\{\text{CONSISTENT},\text{INCONSISTENT}\}$, produced by thresholding the scalar consistency score $S_{\mathrm{EMCR}}$. The per-class performance tables in Section~\ref{sec:Results and Discussion} report $\hat{c}$; the confidence-aware verification analysis (Section~\ref{sec:threshold_analysis}) reports $\hat{y}$.

That's all three placements for #1/#2. Take your time applying them — let me know when you're done and we'll move to #3
