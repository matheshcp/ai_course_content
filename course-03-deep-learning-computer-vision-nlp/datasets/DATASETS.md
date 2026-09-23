# Course 3 — Datasets Manifest

Local path prefix: `course-03-deep-learning-computer-vision-nlp/datasets/`

| Local file | Raw GitHub URL | Schema / notes |
|---|---|---|
| `mnist_train_100.csv` | https://raw.githubusercontent.com/makeyourownneuralnetwork/makeyourownneuralnetwork/master/mnist_dataset/mnist_train_100.csv | label + 784 pixels, no header — 100 rows |
| `mnist_test_10.csv` | https://raw.githubusercontent.com/makeyourownneuralnetwork/makeyourownneuralnetwork/master/mnist_dataset/mnist_test_10.csv | same schema — 10 rows |
| `airline-passengers.csv` | https://raw.githubusercontent.com/jbrownlee/Datasets/master/airline-passengers.csv | Month, Passengers — 144 |
| `flights.csv` | https://raw.githubusercontent.com/mwaskom/seaborn-data/master/flights.csv | year, month, passengers |
| `sms.tsv` | https://raw.githubusercontent.com/justmarkham/DAT8/master/data/sms.tsv | label\ttext — 5574 |
| `IMDB-Dataset.csv` | https://raw.githubusercontent.com/SK7here/Movie-Review-Sentiment-Analysis/master/IMDB-Dataset.csv | review, sentiment — 50001 rows (~66MB) |
| `eng.testa` | https://raw.githubusercontent.com/synalp/NER/master/corpus/CoNLL-2003/eng.testa | CoNLL token\tPOS\tchunk\tNER |
| `news_summary.csv` | https://raw.githubusercontent.com/sunnysai12345/News_Summary/master/news_summary.csv | author, date, headlines, read_more, text, ctext — ~4515 |
| `bus.jpg` | https://raw.githubusercontent.com/ultralytics/yolov5/master/data/images/bus.jpg | single street photo |
| `coco.names` | https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names | 80 COCO class names, one per line |
| `esc50.csv` | https://raw.githubusercontent.com/karolpiczak/ESC-50/master/meta/esc50.csv | filename, fold, target, category, esc10, src_file, take |
| `1-*.wav` (7 files) | https://raw.githubusercontent.com/karolpiczak/ESC-50/master/audio/<name>.wav | 44.1kHz mono clips: dog, chirping_birds, rain, crying_baby, clock_tick, sneezing, clapping |
| `ec2_cpu_utilization_825cc2.csv` | https://raw.githubusercontent.com/numenta/NAB/master/data/realAWSCloudwatch/ec2_cpu_utilization_825cc2.csv | timestamp, value — 5-min CPU |
| `ambient_temperature_system_failure.csv` | https://raw.githubusercontent.com/numenta/NAB/master/data/realKnownCause/ambient_temperature_system_failure.csv | timestamp, value — hourly temp |
| `nltk_index.xml` | https://raw.githubusercontent.com/nltk/nltk_data/gh-pages/index.xml | NLTK data package index |

**Lab → dataset map (13 labs):** L1 mnist_train_100 · L2 CIFAR via keras (`cifar10`, document exception) with mnist CSV fallback · L3/L4 bus.jpg + keras CIFAR subset · L5 airline-passengers · L6 sms.tsv (or IMDB-Dataset) · L7 eng.testa · L8 news_summary · L9 flights · L10/L11 bus.jpg + coco.names · L12 esc50.csv + 7 wavs · L13 NAB CSVs.
