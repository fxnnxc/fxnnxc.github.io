Diversity is all you need

https://github.com/alirezakazemipour/DIAYN-PyTorch


* model.py 
    * Discriminator 
    * ValueNetwork
    * QvalueNetwork
    * PolicyNetwork 
* buffer.py
    * 가득 차면 pop 하는 형태로 제거 

```python
# Add
self.memory.add(state, z, done, action, next_state)

# Save in the buffer 
Transition = namedtuple('Transition', ('state', 'z', 'done', 'action', 'next_state'))
self.buffer.append(Transition(*transition))
if len(self.buffer) > self.buffer_size:
    self.buffer.pop(0)
```

* agent.py : Scope : 학습과 행동 / 모델도 가지고 있기 
 * train 
 * action
// Trainer 가 있는 코드 구현과는 차이가 있다. Trainer의 경우, 각 에이전트는 모델을 가지고 있으며 행동만 하지만, 해당 코드에서는 에이전트가 학습하는 방법까지 가지고 있다. 


* common
    * play.py : agent, env에 대해서 학습된 에이전트를 검증하는 코드 따로 구분 

* main.py
 * 각 애피소드 시작할 때 마다 z 를 샘플링 한다. 


```python
# skills 
n_skills= 3
p_z =  np.full(n_skills, 1/n_skills)
z = np.random.choice(range(n_skills), p=p_z)
```