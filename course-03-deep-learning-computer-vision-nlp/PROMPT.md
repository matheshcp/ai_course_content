# Course 3 — Lab Generation Prompts (13 labs)

Env: 🟢 Colab only (keras/tensorflow preinstalled). Data: see `datasets/DATASETS.md`.

**Global preamble (prepend to every prompt):**

```
Generate a complete self-contained lab as GitHub-flavored Markdown (Jupyter-friendly).
Style contract: H1+H2 + blockquote header (Scenario / You will learn / Time / Level / Needs 🟢);
mental-map table; numbered sections with runnable Python; dataset cell = local datasets/<file> AND raw GitHub URL fallback;
exactly 3 exercises with expected answers + <details> hints; Solutions with runnable code + printed outputs;
"What to learn next"; fixed seeds; outputs under labs/.
Stack: numpy, pandas, matplotlib, tensorflow/keras or PyTorch (pick one and stay consistent — prefer keras for Colab),
scikit-learn for classical baselines. Keep runtimes CPU-friendly (<3 min per training cell) unless noted.
```

---

## Lab 1 — Your First Neural Network (MNIST 100-row warmup)

```
Title: First Neural Network — MNIST Micro-Warmup
Dataset: local datasets/mnist_train_100.csv | raw https://raw.githubusercontent.com/makeyourownneuralnetwork/makeyourownneuralnetwork/master/mnist_dataset/mnist_train_100.csv (label + 784 pixels, no header); mnist_test_10.csv likewise.
Teach: tensors, flatten 28×28, dense ReLU + softmax, sparse categorical crossentropy, 1 epoch on 100 rows just to see the loop; then note full MNIST comes via keras.datasets.
Exercises: (1) reshape and normalise /255; (2) print model.summary() params count; (3) accuracy on the 10-row test file.
Level Beginner ~45min.
```

---

## Lab 2 — CNN on CIFAR-10 (keras built-in)

```
Title: Convolutional Nets — CIFAR-10
Dataset: keras.datasets.cifar10 (document exception: fetched via keras, no local file). Fallback: reuse mnist_train_100 as grayscale CNN demo if offline.
Teach: conv/pool intuition, Conv2D→MaxPool→Dense, data augmentation (flip/shift), ~3-epoch training, test accuracy vs simple MLP baseline.
Exercises: (1) MLP baseline accuracy; (2) CNN accuracy gain; (3) show 5 misclassified images with predicted labels.
Seed 42. Level Intermediate ~60min (3 epochs).
```

---

## Lab 3 — Transfer Learning with a Pretrained CNN

```
Title: Transfer Learning — Feature Extraction
Dataset: keras.datasets.cifar10 subset (2 classes e.g. cat/dog) OR local bus.jpg as single-image demo with MobileNetV2/ImageNet weights.
Teach: why pretrained matters, freeze base, GlobalAveragePooling + head, fine-tune last block optionally.
Exercises: (1) frozen-base accuracy on 2-class subset; (2) unfreeze last 10 layers — accuracy delta; (3) predict bus.jpg classes (top-3 ImageNet labels).
Level Intermediate ~60min.
```

---

## Lab 4 — Image Classification Error Analysis

```
Title: Error Analysis — Where CNNs Break
Dataset: predictions from Lab 2/3 model on cifar10 test subset (regenerate in-notebook); optional local bus.jpg.
Teach: confusion matrix for 10 classes, per-class recall, plotting worst examples, confusable class pairs (cat↔deer etc).
Exercises: (1) 10×10 confusion matrix heatmap; (2) lowest-recall class; (3) 3 concrete data-collection fixes.
Level Intermediate ~45min.
```

---

## Lab 5 — Sequence Forecasting: Airline Passengers

```
Title: Time-Series Forecasting — Airline Passengers
Dataset: local datasets/airline-passengers.csv | raw https://raw.githubusercontent.com/jbrownlee/Datasets/master/airline-passengers.csv (Month, Passengers — 144); secondary flights.csv available.
Teach: train/test split by time (no shuffle!), moving average baseline, simple LSTM or GRU on windows of 12, MAE evaluation, seasonal decomposition intuition.
Exercises: (1) naive seasonal baseline MAE; (2) LSTM MAE vs baseline; (3) forecast next 12 months plot.
Seed 42. Level Intermediate ~60min.
```

---

## Lab 6 — Sentiment Classification (Bag-of-Words → optional LSTM)

```
Title: SMS Sentiment — BoW vs Sequence Models
Dataset: local datasets/sms.tsv | raw https://raw.githubusercontent.com/justmarkham/DAT8/master/data/sms.tsv (alt: IMDB-Dataset.csv for larger run, note 66MB)
Teach: text vectorisation (CountVectorizer / TokenAndVectorize), MultinomialNB baseline, then Embedding+LSTM or keras.TextVectorization+GRU, compare accuracy/F1.
Exercises: (1) NB baseline F1; (2) neural model F1; (3) 5 false positives with tokens that triggered them.
Seed 42. Level Intermediate ~70min.
```

---

## Lab 7 — Named Entity Recognition with Sequence Labelling

```
Title: Named Entity Recognition — CoNLL Format
Dataset: local datasets/eng.testa | raw https://raw.githubusercontent.com/synalp/NER/master/corpus/CoNLL-2003/eng.testa
Format: token POS chunk NER per line; blank line = sentence boundary; -DOCSTART- lines skip.
Teach: BIO tags, loading CoNLL, majority-tag baseline, BiLSTM-CRF sketch (or sklearn CRF suite if available), entity-level metrics.
Exercises: (1) tag distribution; (2) baseline accuracy (usually high — explain why misleading); (3) precision/recall for PER entity type.
Level Advanced ~75min.
```

---

## Lab 8 — Text Summarisation (extractive)

```
Title: Extractive News Summarisation
Dataset: local datasets/news_summary.csv | raw https://raw.githubusercontent.com/sunnysai12345/News_Summary/master/news_summary.csv (headlines, text, ctext)
Teach: sentence tokenisation, TextRank / LSA / simple TF-IDF centroid scoring, ROUGE-L approximation vs reference summary.
Exercises: (1) TF-IDF centroid top-3 sentences; (2) ROUGE-L vs `ctext` on 50 articles; (3) failure case where extractive summary misses the point.
Level Intermediate ~60min.
```

---

## Lab 9 — Sequence-to-Sequence Style Demo (headline generation sketch)

```
Title: Headline Generation Sketch (seq2seq intuition)
Dataset: reuse news_summary.csv (headlines as target, first sentence of text as source) — subsample 5k for CPU.
Teach: encoder-decoder idea, teacher forcing, attention intuition (diagram + small demo, not full production s2s), BLEU-ish score sketch.
Exercises: (1) prepare 5k parallel pairs; (2) train tiny GRU autoencoder-style model 3 epochs; (3) generate 5 headlines, human-rate quality.
Level Advanced ~90min. Seed 42.
```

---

## Lab 10 — Object Detection Concepts + YOLO Inference

```
Title: Object Detection — Run YOLOv5 on bus.jpg
Dataset: local datasets/bus.jpg | raw https://raw.githubusercontent.com/ultralytics/yolov5/master/data/images/bus.jpg ; labels from datasets/coco.names | raw https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names
Teach: detection vs classification, boxes/IoU/mAP intuition, run ultralytics yolov5 (or torch hub) inference, parse results, draw boxes, map class ids via coco.names.
Exercises: (1) list detected objects + confidences; (2) filter conf≥0.5; (3) save labs/bus_detections.jpg.
Level Intermediate ~45min (model download ~100MB once).
```

---

## Lab 11 — Training a Tiny Detector / Custom-Set Inspection

```
Title: Detection Training Loop (tiny custom subset)
Dataset: bus.jpg + a handful of images assembled via keras coco subset OR document transfer: fine-tune YOLOv5s on 20-image toy set for demo; if too heavy, do a guided walkthrough with frozen weights + data-pipeline construction only.
Teach: annotation format (YOLO txt), dataloader, loss terms (obj/cls/box), one overfit-batch demo (memorise 1 batch) as sanity check.
Exercises: (1) build dataloader yielding one batch; (2) overfit 1 batch loss↓; (3) list 3 reasons full training is expensive.
Level Advanced ~75min.
```

---

## Lab 12 — Audio Classification with ESC-50 Samples

```
Title: Environmental Sound Classification (ESC-50 sample)
Dataset: local datasets/esc50.csv | raw https://raw.githubusercontent.com/karolpiczak/ESC-50/master/meta/esc50.csv ; 7 wavs in datasets/: 1-100032-A-0.wav (dog), 1-100038-A-14.wav (chirping_birds), 1-100210-A-36.wav & 1-100210-B-36.wav (rain), 1-101296-A-19.wav (crying_baby), 1-101336-A-30.wav (clock_tick), 1-101404-A-34.wav (clapping)
Raw audio base: https://raw.githubusercontent.com/karolpiczak/ESC-50/master/audio/<filename>
Teach: waveform → spectrogram (librosa or scipy.stft), MFCC features, small CNN or kNN classifier on the 7 clips (few-shot demo), audio normalisation.
Exercises: (1) plot spectrogram for dog vs rain; (2) MFCC mean vector per clip; (3) leave-one-out accuracy on 7 clips.
Level Intermediate ~55min.
```

---

## Lab 13 — Anomaly Detection on Multivariate/Time Series (NAB)

```
Title: Time-Series Anomaly Detection (NAB)
Dataset: local datasets/ec2_cpu_utilization_825cc2.csv | raw https://raw.githubusercontent.com/numenta/NAB/master/data/realAWSCloudwatch/ec2_cpu_utilization_825cc2.csv ; local datasets/ambient_temperature_system_failure.csv | raw https://raw.githubusercontent.com/numenta/NAB/master/data/realKnownCause/ambient_temperature_system_failure.csv (both: timestamp, value)
Teach: rolling statistics, z-score / IQR detectors, simple isolation forest on window features, point-adjust evaluation, visualising alarm regions.
Exercises: (1) z-score alarms on CPU series; (2) count anomalies before/after rolling median filter; (3) compare detectors on ambient temp.
Level Intermediate ~60min. Seed 42.
```
