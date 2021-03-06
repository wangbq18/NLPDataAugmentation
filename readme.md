# NLP Data Augmentation
主要对SLU数据做数据增强，主要包括意图分类数据、槽位填充的数据。理论上分类数据也行。

将[eda](https://arxiv.org/abs/1901.11196)中方法使用语言模型做增强，而非随机替换
## 随机插入

> 见 [bert_main.py](./bert_main.py)

将句子中随机选取位置插入词语，通过使用Bert来预测插入位置概率最大的词语。例如：

原始句子：
```
帮我查一下航班信息
查一下航班信息
附近有什么好玩的
```

生成的句子：
```text
请帮我查一下航班信息
能帮我查一下航班信息
你帮我查一下航班信息
检查一下航班信息
调查一下航班信息
查查一下航班信息
这附近有什么好玩的
那附近有什么好玩的
家附近有什么好玩的
附近附有什么好玩的
附近最有什么好玩的
附近近有什么好玩的
```
更多生成的结果查看[./data/bert_output](./data/bert_output)

[eda](https://arxiv.org/abs/1901.11196)方法：

- synonym replacement(SR)：随机选取句子中n个非停用词的词语。对于每个词语随机选取它的一个同义词替换该词语。
- random insertion(RI)：随机选取句子中的一个非停用词的词语，随机选取这个词语的一个近义词，将近义词随机插入到句子中，做n次。
- random swap(RS)：随机选取两个词语，交换他们的位置，做n次。
- random deletion(RD)：对于句子中的每个词语，以概率p选择删除。

[eda中文解读](https://blog.csdn.net/shine19930820/article/details/103789604)

# 词语丰富
用于丰富ontology，针对词语进行丰富，包括：

- 使用词向量做相似词语的召回，丰富ontology。
- 使用wordNet做近义词召回。

## 词向量召回

> 见 [word_sim.py](./word_sim.py)

例如现有实体空调，需要召回空调的一些泛化说法、同义词等，召回同义词：

```
[('冷气', 0.832690954208374), ('暖气', 0.7806607484817505), ('电扇', 0.7694630026817322), ('电热', 0.7415034174919128), ('风扇', 0.7370954751968384), ('供暖', 0.7363734841346741), ('采暖', 0.7239724397659302), ('电暖', 0.7215089797973633), ('通风', 0.7174738645553589), ('隔音', 0.7118726968765259)]

```

## wordNet | 同义词库

同样对于实体词=空调，召回结果：

wordNet:

```
['冷气机', '空调', '空调器', '空调装置', '空调设备']
```

同义词（synonyms）

```
(['空调', '冷气', '空调设备', '空调系统', '波箱', '用车', '制冷', '空调机', '空气调节', '巴士在'], [1.0, 0.75175405, 0.7452018, 0.6877022, 0.6544307, 0.62812567, 0.62259305, 0.59779996, 0.57414114, 0.5611771])

```





# reference
- [EDA: Easy Data Augmentation Techniques for Boosting Performance on Text Classification Tasks](https://arxiv.org/abs/1901.11196)
- https://github.com/jasonwei20/eda_nlp
- https://ai.tencent.com/ailab/nlp/en/embedding.html
- https://mp.weixin.qq.com/s/qmJdioUgqcahc_9RLn9vjw
- Conditional BERT Contextual Augmentation
