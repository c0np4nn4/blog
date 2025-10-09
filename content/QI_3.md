# Quantum Algorithms
## Classical Computations on a Quantum Computer
양자 컴퓨터는 고전적 계산을 시뮬레이션할 수 있다. 이번 장에서는 양자 컴퓨터가 고전적 계산에 비해 극적인 향상(dramatical improvements) 을 달성할 수 있음을 보여주는 예시를 살펴본다. 

양자 컴퓨터 상에서 계산을 수행하기 위해서는 기존 회로를 대응되는 양자 회로로 변환하는 작업이 필요하다. 이 때, 고전 회로의 "비가역성"을 이해하는 것이 중요하다. 예를 들어, 논리 회로 중 `AND` 게이트를 생각해보자. 이 게이트의 출력은 하나의 비트이며, 출력값만으로는 각 입력값을 완전히 결정할 수 없다. 예를 들어 출력 비트가 $0$이라면, 입력 비트 쌍은 $(\mathsf{in}_1, \mathsf{in}_2) \in \lbrace(0,0), (1,0), (0,1)\rbrace$ 이다.
양자 회로는 "가역적"인 구조이므로, 이를 위해 $\mathsf{Toffoli}$ 게이트를 이용해 가역적인 구조를 만들면 된다.

### Toffoli Gate
$\mathsf{Toffoli}$ 게이트는 3-input bits, 3-output bits로 아래와 같이 구성된다.

|    input     |                  output                  |
| :----------: | :--------------------------------------: |
| $\mathbf{a}$ |               $\mathbf{a}$               |
| $\mathbf{b}$ |               $\mathbf{b}$               |
| $\mathbf{c}$ | $\mathbf{c} \oplus \mathbf{a}\mathbf{b}$ |

이 때, $\mathbf{a}\mathbf{b}$ 는 `AND` 게이트의 결과로 생각하면 된다. $\mathbf{a}$, $\mathbf{b}$는 각각 *control bit* 이고 $\mathbf{c}$는 *target bit* 이다. $\mathsf{Toffoli}$ 게이트를 두 번 적용하면 처음의 상태로 돌아오기 때문에 "가역적(reversible)"이다. 

만약 `NAND` 게이트를 시뮬레이션 하고 싶으면 $\mathbf{c} = 1$ 로 두면 된다. 결과를 살펴보면 아래와 같다.

| $\mathbf{a}$ | $\mathbf{b}$ | $\mathbf{c}$ | $\mathbf{c} \oplus \mathbf{a}\mathbf{b}$ |       $\neg (\mathbf{a}\mathbf{b})$ (`NAND`)        |
| :----------: | :----------: | :----------: | :--------------------------------------: | :-------------------------------------------------: |
| $\mathsf{0}$ | $\mathsf{0}$ | $\mathbf{1}$ |               $\mathsf{1}$               | $\neg(\mathsf{00}) = \neg(\mathsf{0}) = \mathsf{1}$ |
| $\mathsf{0}$ | $\mathsf{1}$ | $\mathbf{1}$ |               $\mathsf{1}$               | $\neg(\mathsf{01}) = \neg(\mathsf{0}) = \mathsf{1}$ |
| $\mathsf{1}$ | $\mathsf{0}$ | $\mathbf{1}$ |               $\mathsf{1}$               | $\neg(\mathsf{10}) = \neg(\mathsf{0}) = \mathsf{1}$ |
| $\mathsf{1}$ | $\mathsf{1}$ | $\mathbf{1}$ |               $\mathsf{0}$               | $\neg(\mathsf{11}) = \neg(\mathsf{1}) = \mathsf{0}$ |

이런 경우 세 번째 비트 ($\mathbf{c}$)를 `ancilla state` 라 부른다.

하나의 예를 더 살펴보면, 첫 번째 비트와 세 번째 비트($\mathbf{a}, \mathbf{c}$)를 각각 $\mathsf{1}$, $\mathsf{0}$의 standard ancilla state로 두면 두 번째 비트($\mathbf{b}$)를 여러 출력값으로 얻는 `FANOUT` 회로를 만들 수도 있다.

| $\mathbf{a}_{\mathsf{in}}$ | $\mathbf{b}_{\mathsf{in}}$ | $\mathbf{c}_{\mathsf{in}}$ | $\mathbf{a}_{\mathsf{out}}$ | $\mathbf{b}_{\mathsf{out}}$ | $\mathbf{c}_{\mathsf{out}}$                                                                                                                                         |
| :------------------------: | :------------------------: | :------------------------: | :-------------------------: | :-------------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|        $\mathbf{1}$        |        $\mathsf{0}$        |        $\mathbf{0}$        |        $\mathsf{1}$         |        $\mathbf{0}$         | $\mathbf{c}_\mathsf{in} \oplus \mathbf{a}_\mathsf{in}\mathbf{b}_\mathsf{in} = \mathsf{0} \oplus (\mathsf{1}\mathsf{0}) = \mathsf{0} \oplus \mathsf{0} = \mathbf{0}$ |
|        $\mathbf{1}$        |        $\mathsf{1}$        |        $\mathbf{0}$        |        $\mathsf{1}$         |        $\mathbf{1}$         | $\mathbf{c}_\mathsf{in} \oplus \mathbf{a}_\mathsf{in}\mathbf{b}_\mathsf{in} = \mathsf{0} \oplus (\mathsf{1}\mathsf{1}) = \mathsf{0} \oplus \mathsf{1} = \mathbf{1}$ |

다시 말해, $\mathbf{b}_\mathsf{in} = \mathsf{b}_\mathsf{out} = \mathsf{c}_\mathsf{out}$ 이다. 하나의 input 값이 두 개의 output 으로 나오는 게이트로 이해할 수 있다.

이처럼 $\mathsf{Toffoli}$ 게이트를 이용하면 고전 연산을 수행하던 고전 컴퓨터 회로들을 양자 회로로 변환할 수 있으므로, 양자 회로 상에서의 계산으로 변환할 수 있다.

---

## Quantum Parallelism
양자 알고리즘에서 `Parallelism`은 중요한 특징 중 하나이다. 이를 이용하면 임의의 함수 $f(x)$에 대해, 다수의 서로 다른 입력값 $x$에서의 평가값을 동시에 계산하는 것이 가능하다.
예를 들어, $f(x) : \lbrace 0, 1 \rbrace \mapsto \lbrace 0, 1 \rbrace$ (one-bit domain and range; 하나의 비트를 정의역, 치역으로 하는 함수)를 가정하자. 이 함수를 양자 회로로 적절히 (e.g., $\mathsf{Toffoli}$ 게이트를 이용하여)변환한 결과를 $U_f$ 로 두면, 아래와 같이 나타낼 수 있다.

$$
U_f : |x, \,y\rangle \mapsto |x, \,y \oplus f(x) \rangle
$$
이 때, 첫 번째 위치는 "data register" 이고 두 번째 위치는 "target register"이다. $y=0$ 이면 두 번째 위치가 단순히 $f(x)$가 됨을 쉽게 알 수 있다.

(wip)