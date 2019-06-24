## Deep Learning to Solve Challenging Problems (Google I/O'19)
[link](https://www.youtube.com/watch?v=rP8CGyDbxBY&list=PLOU2XLYxmsIJ5Bl3HmuxKY5WE555cu9Uc&index=5&t=3s)
1. The number of papers posted related to machine learning grows exponentially in the past years since 2009
2. In imagenet contest, methods using deep learning ahcives much better results than those without DL methods
3. Waymo: DL helps understand the environment around cars better
4. Improve grasp success rate of robot (from 65% to 96%)
5. DL in the area of self-supervised imitation learning (let robot imitate human being, RL involved)
6. DL in health informatics such as regular screening of disease
7. Novel scientific discoveries (e.g. predict gender, age through eye image)
8. Tensorflow: tool to express machine learning ideas. Computation is defined as graph

__________________

### Ongoing Research
1. Bigger ML models, but sparsely activated. For instance, 1000 models and only 20 activated at a certain point. Idea is MoE layer (expert) inserted into each neuron network layer as a gating network. Train the routing network to determine which network to be activated.
2. AutoML: automated ML. Current = ML epertise + data + computation. Can we turn this into solution = data + computation? Idea is model-generating model trained via RL. For instance, we generate 10 models, train them for a few hours and use loss of the generated models as RL signal (reward). Repeat the step again and again. AutoML outperforms handcrafted models. You can get slightly more accurate model with high computation cost or much accurate model with low computation cost. **Already deployed in cloud AutoML** . It has models in vision, video intelligence, natural language, translation, tables (for structured data). But still need lots of research onging to improve performance. More computation power is needed.
3. 

