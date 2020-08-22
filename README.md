# Artificial Intelligence
In this repository, I will be posting technical papers which I found useful while working on AI and a brief summary about it.

1. [**Plan Explanations as Model Reconciliation: Moving Beyond Explanation as Soliloquy**](https://www.ijcai.org/Proceedings/2017/0023.pdf)

The paper discusses providing an explanation to humans by AI systems. The explanation is considered a “model reconciliation problem” (MRP), where the AI system suggests changes to the human model, to make its plan optimal for the changed human model. The need arises due to the differences in the model of human and AI system. There are two ways to tackle this problem. The first is to make the AI system “explicable”. But this puts additional constraints on the AI system. The second approach is to make minimal changes to the human model to make it closer to the AI model with the same optimal plan. The plan is stored in PDDL format. The explanation can have characteristics like complete, concise, monotonic, and computable. To compute explanations, A* search is used.

Model is defined as <D,I,G>: D is domain = <F, A>, F is set of fluent the define world state, A is set of actions, I is initial state and G is goal state. Action is a tuple consisting of <ca, pre(a), eff+(a), eff-(a)> where ca denotes cost, pre(a) is preconditions and eff+(a)/eff-(a) are add/delete effects 

Multi-Model Planning (MMP) setting consists of <MR, MH> where MR is the model of the planner, MH is the human’s approximation of the planner’s model. Model Reconciliation Problem (MRP) consists of both the models and an optimal in MR. The solution to MRP is called Multi-Model Explanation which consists of a series of edit functions that can transform MH to a model in which the plan of MRP is closer to being optimal. Model change actions (edit function) can only make a single change to a domain at a time, and all these changes are consistent with the model of the planner.  

There are different ways to compute an explanation:

·     Patch Plan Explanation (PPE): Provide model differences to only the actions that are present in the plan that needs to be explained. This is incomplete

·     Model Patch Explanation (MPE): Provide entire model differences to the humans. Less concise

·     Minimally Complete Explanation (MCE): Search the space of the model and expose only relevant information. It gives the smallest domain change that may be made to make the given plan optimal in the updated action model. 

The model space for MCE is searched with A*. The search is started with an initial state representing the human model. The search continues until we arrive at a model in which the robot plan is optimal (the plan is explained). The priority queue to get the successor nodes is such that the model changes that are relevant in the robot model and the human model are popped first. Model changes are represented as set. Cost is attached to each edit function to capture differential importance. 

2. [**Augmenting Knowledge through Statistical, Goal-oriented Human-Robot Dialog**](https://arxiv.org/pdf/1907.03390.pdf)

The paper discusses a multitask dialog management problem, where a robot simultaneously identifies service requests through a human-robot dialog and learns new entities from this experience to augment its internal knowledge base (KB). The robot dialog system consists of three components for language understanding (state tracking, dialog management, and language synthesis) and one for knowledge augmentation. The work is focused on dialog-based robot knowledge augmentation, where the agent must identify when it is necessary to augment its KB and where in the KB to do that.

Dialog agent consists of two interleaved control loops resulting in a dual-track controller. One track focuses on maintaining the dialog belief state and suggests language actions to the agent. The other focuses on maintaining the belief of the current knowledge being (in)sufficient to complete the task and suggests knowledge augmentation. Dialog Management Track and Knowledge Management Track are modeled as POMDP. The tracks are separated because the knowledge track is only used for the purpose of maintaining beliefs (not for action selection). Modeling the uncertainty of unknown entities in a single controller will result in unnecessarily long dialogs. Separating the two tracks reduces the learning complexity of the entire framework. For understanding the natural language and make observations for POMDP, a semantic parser based on Semantic Parsing Framework (SPF) is used. Using the semantic parser, the request in natural language is transformed to a formal representation compatible with the robots internal KB. If the language understanding fails, then a random observation is made. The natural language should be of the form <task, item, recipient>

For representing the knowledge base, Answer Set Prolog (ASP) is used. It is a declarative language for knowledge representation. The entropy is a measure of the uncertainty level of the agent’s belief distribution. When the agent is (un)confident about the state, the entropy value is (high) low. In particular, a uniform belief distribution corresponds to the highest entropy level. If the belief entropy is higher than threshold h (meaning the agent is highly uncertain about the dialog state), the human user is encouraged to state the entire request in one sentence. Otherwise, the dialog manager would decide the flow of the conversation. A belief queue is maintained for the last three dialog turns. Entropy fluctuation occurs if the entropy of the second last is the highest or lowest among the three.

Algorithm for knowledge augmentation

Let M and M+ be the models for dialog-track and knowledge track control respectively. The algorithm starts by initializing the two beliefs with uniform distributions. 

•	A new entity is added to KB if the marginal probability of knowledge belief over recipient or item is greater than the threshold value. Or the number of entropy fluctuations is higher than the threshold

•	If the entropy of dialog belief is higher than belief threshold than ask for rewording the original service request. Else use the dialog POMDP to maintain dialog flow. 

•	Knowledge belief is only updated with confirming questions which invalidate the agent’s hypothesis of unknown entities

•	Return the request based on the last action and (possibly updated) knowledge base

After knowledge base update POMDP is constructed on-the-fly

