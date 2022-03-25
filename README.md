# KG_Enhanced_Wiki_Corpora
This project is implemented for knowledge enhanced pre-trained corpora by Wikipedia and Wikidata5M. If your project needs the pretrained kg-enhanced corpus, you can directly push this project into the root directory of your project.

If you have used this project, please cite me


## Step1: Download wikipedia copora
Step into the directory ```/pretrain_data```, and run:
```shell
wget -c https://dumps.wikimedia.org/enwiki/latest/enwiki-latest-pages-articles.xml.bz2
```
or you can manually download data from [WikiDumps](https://dumps.wikimedia.org/enwiki/) to the directory ```/pretrain_data```.

## Step2: Download wikidata5m knowledge base
Make new directory ```/pretrain_data/kg```, step into this directory , and run:
```shell
wget -c https://www.dropbox.com/s/lnbhc8yuhit4wm5/wikidata5m_alias.tar.gz?dl=1
wget -c https://www.dropbox.com/s/6sbhm0rwo4l73jq/wikidata5m_transductive.tar.gz?dl=1
tar -xzvf wikidata5m_alias.tar.gz
tar -xzvf wikidata5m_transductive.txt.gz
```
or you can manually download from [Wikidata5M](https://deepgraphlearning.github.io/project/wikidata5m).

You will obtain some files:
- wikidata5m_transductive_train.txt
- wikidata5m_transductive_valid.txt
- wikidata5m_transductive_test.txt
- wikidata5m_entity.txt
- wikidata5m_relation.txt

## Step3: Extract text from semi-structure corpora
The original wikipedia corpus consists much more semi-structure tokens, such as <doc id=xx></doc>, <a href=xx>/<a>. Run the following command and obtain the extracted corpus:
```shell
python3 preprocess/WikiExtractor.py pretrain_data/enwiki-latest-pages-articles.xml.bz2 -o pretrain_data/output -l --min_text_length 100 --filter_disambig_pages -it abbr,b,big --processes 4
```
You wil obtain the directory ```output/```.

## Step4: Extract entity mention from corpora
We process the text, and extract the corresponding entity mention from all ``<a></a>'':
```shell
python3 preprocess/extract.py  
```
Then, you will obtain the directory ```ann/```.

## Step5: Align Wikidata5M with the corpus, and obtain tokenized text
Run the following command, and you will obtain the final-finished corpus:
```shell
python3 preprocess/gen_data.py  
```
You will obtain ```data/```. The corpus file is ```data.json```.

An example of processed data for each line:
```text
{"token_ids": [0, 1121, 29690, 3221, 9, 5, 5444, 6, 10, 16192, 16, 2128, 156, 227, 5, 2, 0, 36675, 3937, 26399, 8, 18030, 2459, 811, 2, 0, 11850, 11418, 293, 147, 5, 320, 16, 2273, 19, 13223, 9594, 645, 8, 5, 5442, 19, 16529, 11140, 4, 2, 0, 597, 10382, 5691, 46708, 2, 0, 5384, 6796, 14, 10, 24904, 9, 5, 80, 21, 144, 21453, 4, 1437, 33251, 6393, 2, 0, 22591, 24652, 2, 0, 18, 2, 0, 36675, 38520, 46186, 2, 0, 12597, 40826, 99, 37, 794, 25, 5, 31779, 11, 82, 7, 81, 12, 2544, 44611, 1496, 8, 3014, 3722, 4472, 4, 96, 980, 15801, 6, 5, 7571, 29, 8, 6200, 29, 2, 0, 34665, 2, 0, 28644, 13, 35065, 8, 6684, 21904, 15, 5, 5648, 21, 1440, 71, 2, 0, 36675, 38520, 2, 0, 6, 30, 2, 0, 34665, 2, 0, 37096, 2, 0, 250, 1610, 5344, 7864, 2, 0, 35, 22, 36675, 38520, 5793, 39, 16224, 13214, 420, 5, 2083, 21, 3901, 7, 5, 2821, 3189, 9, 5, 1850, 586, 4, 2], "entity_qid": ["Q618929", "Q9358", "Q41532", "Q4780365", "Q23548", "Q46611", "Q23548", "Q2774189"], "entity_pos": [[17, 25], [47, 52], [70, 73], [77, 81], [115, 117], [132, 135], [140, 142], [146, 151]], "relation_pid": null, "relation_pos": null}
```
- token_ids: The input token_id tokenized by Roberta-based from huggingface
- entity_qid: The entity id aligned with Wikidata5M
- entity_pos: The entity mention span position
- relation_pid: Default as null, you can individually desgin it.
- relation_pos: Default as null, you can individually desgin it.

---
If you have used this reporisty, please cite me:
```
@misc{Wang2022,
  author = {Jianing Wang},
  title = {KG_Enhanced_Wiki_Corpora},
  year = {2022},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/wjn1996/KG_Enhanced_Wiki_Corpora}},
}
```
  
  
