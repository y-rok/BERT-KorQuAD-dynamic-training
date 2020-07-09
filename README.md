# Dynamic Training 

- (인라이플 공개 소스)https://github.com/enlipleai/kor_pretrain_LM 으로 부터 소스 수정
- 기존 소스에서 BERT로 [KorQuADv1.0](https://korquad.github.io/category/1.0_KOR.html) 학습시 문제 없지만, [AI Hub의 News Q&A](http://www.aihub.or.kr/aidata/86)와 같은 대용량 데이터 학습시 RAM이 부족
  - 이는 학습데이터를 모두 Input Feature로 변환하여 pkl로 저장하는데 Input Feature 전체를 RAM에 올리기 때문에 발생하는 문제
- 이를 해결하기 위해, 동적으로 학습데이터를 전처리하여 Feature를 만드는 방식 구현

## Example Scripts
**KorQuAD1.0**

# * Train

- 추가된 Arguments
   - --dynamic_training - dynamic training 활성화
   - --num_workers n - n개의 worker로 학습 데이터 전처리
   
```shell
python3 run_qa.py \
  --dynamic_training
  --num_workers 2
  --checkpoint $MODEL_FILE \
  --config_file $CONFIG_FILE \
  --vocab_file $VOCAB_FILE \
  --train_file data/korquad/KorQuAD_v1.0_train.json \
  --max_seq_length 512 \
  --doc_stride 128 \
  --max_query_length 64 \
  --max_answer_length 30 \
  --per_gpu_train_batch_size 16 \
  --learning_rate 5e-5 \
  --num_train_epochs 4.0 \
  --adam_epsilon 1e-6 \
  --warmup_proportion 0.1
```

# * Eval
```shell
python3 eval_qa.py \
  --checkpoint $MODEL_FILE \
  --config_file $CONFIG_FILE \
  --vocab_file $VOCAB_FILE \
  --predict_file data/korquad/KorQuAD_v1.0_dev.json \
  --max_seq_length 512 \
  --doc_stride 64 \
  --max_query_length 64 \
  --max_answer_length 30 \
  --batch_size 16 \
  --n_best_size 20
```
