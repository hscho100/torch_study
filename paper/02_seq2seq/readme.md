## 🤗 Result
- 

## 🤔 Paper review
### 1) PPT 한 장 분량으로 자유롭게 논문 정리 뒤 이미지로 첨부

### 2) (슬랙으로 이미 토론을 했지만 그래도) 이해가 안 가는 부분, 이해가 안 가는 이유(논문 본문 복붙)

= 우리의 objective function는 ? 

계속 말하고 있는 perplexity가 모든 t시점의 softmax 결과값인 logSoftmax loss를 다 더한것이겠지? → ㅇㅇ 맞다
= 논문에서 임베딩 시각화한 것 :  Thus the deep LSTM uses 8000 real number to represnt a sentence에서 8000은 1000(=hidden cell dim) * 4(=num layers of LSTM)  * 2(=hidden, cell state) 인건 알겠는데 PCA할때 그냥 concat했으려나 아님 (1000 by 4 by 2) 를 PCA? 후자일듯?

= long minimal time lag 

그냥 ..long term dependency 인듯..
Traditional recurrent nets fail in case of long minimal time lags between input signals and corresponding error signals 
Hence, such a network architecture can never learn the relation between actions that it has chosen in the beginning and the consequences much later. This is called the 'vanishing gradient problem' and this is what they mean by 'lag' in the LSTM paper because LSTMs where precisely developed to overcome this exact problem (by letting the information flow in a more controlled way backwards somewhat but I never really understood how that exactly works to be honest :-))
( [https://stats.stackexchange.com/questions/443172/what-is-time-lag-in-recurrent-neural-network-why-is-it-a-problem](https://stats.stackexchange.com/questions/443172/what-is-time-lag-in-recurrent-neural-network-why-is-it-a-problem) )
= SMT에서 rescore?

SMT 에서 나온 결과물을 개선시키기 위해 SMT에서 뽑힌 1000개의 리스트를 다시 평가시키는 것..?

Finally, we used the LSTM to rescore the publicly available 1000-best lists of the SMT baseline on
the same task [29]. By doing so, we obtained a BLEU score of 36.5, which improves the baseline by
3.2 BLEU points and is close to the previous best published result on this task (which is 37.0 [9]).

Various approaches have been proposed over
the past decade for the purpose of improving the
phrase pair quality for SMT. For example, a term
weight based model was presented in (Zhao, et
al., 2004) to rescore phrase translation pairs. It
models the translation probability with similarities between the query (source phrase) and
document (target phrase).  ( [http://mt-archive.info/Coling-2010-Huang-1.pdf](http://mt-archive.info/Coling-2010-Huang-1.pdf) )

### 3) 재밌었던 부분

= reversed : 나중에 RNN seq2seq쓰는 것 있음 실험해봐야겠다 싶었음! 
= PCA해봤더니 단어는 거의 비슷한데 의미는 반대인 것이 묶인 것. 결과가 꽤 놀라워서 체리피킹인지 아닌지 꼭 테스트를 해봐야겠다
= softmax 구하는데만 4개의 GPU를 쓴 것...ㅋㅋ→ 그럼 요즘은 다 hierachial softmax 쓰는건가?
= size가 1인 beam search 즉 greedy search 성능이 나쁘지 않았던 것. 내가 seq2seq + greedy search를 썼을 땐 계속 확률이 높은 것 같은 똑같은 단어를 반복하던데 그런건 BLEU에서 크게 penalty 되지 않아서일까? 아니면 내가 트레이닝을 이상하게 시켜서 그런걸까 
= We found deep LSTMs to significantly outperform shallow LSTMs, where
each additional layer reduced perplexity by nearly 10% → 더 깊은게 항상 좋은건 아닌데 이 경우엔 깊은게 훨씬 좋았다네..그냥 신기

### 4) 논문 구현 시 주의해야할 것 같은 부분(논문 본문 복붙)

= most frequent 단어만 사용하고 나머지는 [UNK] 처리함 → 결국 corpus 한 바퀴 다 봐야함ㅎㅎ

We used 160,000 of the most frequent words for the source language
and 80,000 of the most frequent words for the target language. Every out-of-vocabulary word was
replaced with a special “UNK” token.
= LSTM weight uniform 초기화

We initialized all of the LSTM’s parameters with the uniform distribution between -0.08
and 0.08
= beam search decoder 
We search for the most likely translation using a simple left-to-right beam search decoder
= exploding gradient를 피하기 위하여 L2 norm gradient clipping

Thus we enforced a hard constraint on the norm of the gradient [10,
25] by scaling it when its norm exceeded a threshold.
= 비슷한 길이 애들끼리 묶어줘야함! 

To address this problem, we made sure
that all sentences in a minibatch are roughly of the same length, yielding a 2x speedup.

## 🤫 논문과 다르게 구현한 부분
-

## 🤭 논문 구현하면서 배운 점 / 느낀 점
-
