# Artificial Intelligence
In this repository, I will be posting technical papers which I found useful while working on AI and a brief summary about it.

[**Plan Explanations as Model Reconciliation: Moving Beyond Explanation as Soliloquy**] (https://www.ijcai.org/Proceedings/2017/0023.pdf)

The paper discusses providing an explanation to humans by AI systems. The explanation is considered a “model reconciliation problem” (MRP), where the AI system suggests changes to the human model, to make its plan optimal for the changed human model. The need arises due to the differences in the model of human and AI system. There are two ways to tackle this problem. The first is to make the AI system “explicable”. But this puts additional constraints on the AI system. The second approach is to make minimal changes to the human model to make it closer to the AI model with the same optimal plan. The plan is stored in PDDL format. The explanation can have characteristics like complete, concise, monotonic, and computable. To compute explanations, A* search is used.

Model is defined as <D,I,G>: D is domain = <F, A>, F is set of fluent the define world state, A is set of actions, I is initial state and G is goal state. Action is a tuple consisting of <ca, pre(a), eff+(a), eff-(a)> where ca denotes cost, pre(a) is preconditions and eff+(a)/eff-(a) are add/delete effects 

Multi-Model Planning (MMP) setting consists of <MR, MH> where MR is the model of the planner, MH is the human’s approximation of the planner’s model. Model Reconciliation Problem (MRP) consists of both the models and an optimal in MR. The solution to MRP is called Multi-Model Explanation which consists of a series of edit functions that can transform MH to a model in which the plan of MRP is closer to being optimal. Model change actions (edit function) can only make a single change to a domain at a time, and all these changes are consistent with the model of the planner.  

There are different ways to compute an explanation:

·     Patch Plan Explanation (PPE): Provide model differences to only the actions that are present in the plan that needs to be explained. This is incomplete

·     Model Patch Explanation (MPE): Provide entire model differences to the humans. Less concise

·     Minimally Complete Explanation (MCE): Search the space of the model and expose only relevant information. It gives the smallest domain change that may be made to make the given plan optimal in the updated action model. 

The model space for MCE is searched with A*. The search is started with an initial state representing the human model. The search continues until we arrive at a model in which the robot plan is optimal (the plan is explained). The priority queue to get the successor nodes is such that the model changes that are relevant in the robot model and the human model are popped first. 

 Model changes are represented as set. Cost is attached to each edit function to capture differential importance. 