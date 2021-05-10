# Paraphrase Data

**This page is currently work-in-progress and will be extended in the future**

In our paper [Making Monolingual Sentence Embeddings Multilingual using Knowledge Distillation](https://arxiv.org/abs/2004.09813) we showed that paraphrase dataset together with [MultipleNegativesRankingLoss](https://www.sbert.net/docs/package_reference/losses.html#multiplenegativesrankingloss) is a powerful combination to learn sentence embeddings models.

You can find here: [NLI - MultipleNegativesRankingLoss](https://www.sbert.net/examples/training/nli/README.html#multiplenegativesrankingloss) more information how the loss is be used.

In this folder, we collect different datasets and scripts to train using paraphrase data.

## Datasets

You can find here: [sbert.net/datasets/paraphrases](http://sbert.net/datasets/paraphrases) a list of datasets with paraphrases suitable for training.

| Name | Source | #Sentence-Pairs | STSb-dev |
| --- | --- | :---: | :---: |
| [AllNLI.tsv.gz](https://public.ukp.informatik.tu-darmstadt.de/reimers/sentence-transformers/datasets/paraphrases/AllNLI.tsv.gz) | [SNLI](https://nlp.stanford.edu/projects/snli/) + [MultiNLI](https://cims.nyu.edu/~sbowman/multinli/) | 277,230 | |
| [altlex.tsv.gz](https://public.ukp.informatik.tu-darmstadt.de/reimers/sentence-transformers/datasets/paraphrases/altlex.tsv.gz) | [altlex](https://github.com/chridey/altlex/) | 1123696 |
| [coco_captions.tsv.gz](https://public.ukp.informatik.tu-darmstadt.de/reimers/sentence-transformers/datasets/paraphrases/coco_captions.tsv.gz) | [COCO](https://cocodataset.org/) | 828,395 |
| [quora_duplicates.tsv.gz](https://public.ukp.informatik.tu-darmstadt.de/reimers/sentence-transformers/datasets/paraphrases/quora_duplicates.tsv.gz) | [Quora](https://quoradata.quora.com/First-Quora-Dataset-Release-Question-Pairs) | 103,663 | |
| [sentence-compression.tsv.gz](https://public.ukp.informatik.tu-darmstadt.de/reimers/sentence-transformers/datasets/paraphrases/sentence-compression.tsv.gz) | [sentence-compression](https://github.com/google-research-datasets/sentence-compression) | 180,000 | |
| [SimpleWiki.tsv.gz](https://public.ukp.informatik.tu-darmstadt.de/reimers/sentence-transformers/datasets/paraphrases/SimpleWiki.tsv.gz) | [SimpleWiki](https://cs.pomona.edu/~dkauchak/simplification/) | 102,225 |
| [wiki-atomic-edits.tsv.gz](https://public.ukp.informatik.tu-darmstadt.de/reimers/sentence-transformers/datasets/paraphrases/wiki-atomic-edits.tsv.gz) | [wiki-atomic-edits](https://github.com/google-research-datasets/wiki-atomic-edits) |   22,980,185
| [wiki-split.tsv.gz](https://public.ukp.informatik.tu-darmstadt.de/reimers/sentence-transformers/datasets/paraphrases/wiki-split.tsv.gz) | [wiki-split](https://github.com/google-research-datasets/wiki-split) | 929,944

All datasets have a sample per line and the individual sentences are seperated by a tab (\t). Some datasets (like AllNLI) has three sentences per line: An anchor, a positive, and a hard negative.

We measure for each dataset the performance on the STSb dataset after 10k training steps with a distilroberta-base model and a batch size of 256.

## Training
See [training.py](training.py) for the training script.

The training script allows to load one or multiple files. We construct batches by sampling examples from the respective dataset. So far, examples are not mixed between the datasets, i.e., a batch consists only of examples from a single dataset.

As the dataset sizes are quite different in size, we perform a tempurate controlled sampling from the datasets: Smaller datasets are up-sampled, while larger datasets are down-sampled. This allows an effective training with very large and smaller datasets.