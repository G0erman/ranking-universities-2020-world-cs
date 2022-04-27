# World Top CS Universities [2020]

## Import Modules


```python
import csv
from typing import List
```

## Read Rankings

Our data sources are the following:

* [QS: World University Rankings 2020 — Computer Science & Information Systems](https://www.topuniversities.com/university-rankings/university-subject-rankings/2020/computer-science-information-systems)
* [Times Higher Education: World University Rankings 2020 — Computer Science](https://www.timeshighereducation.com/world-university-rankings/2020/subject-ranking/computer-science)
* [Shanghai Ranking Consultancy: Academic Ranking of World Universities 2019 — Computer Science & Engineering](http://www.shanghairanking.com/Shanghairanking-Subject-Rankings/computer-science-engineering.html)



```python
def read_tsv(path: str) -> List:
    with open(path) as tsv_file:
        reader = csv.reader(tsv_file, delimiter="\t")
        lines = list(reader)[:50]  # We'll stick to the top-50 institutions

        return lines

ranking_the = read_tsv("data/ranking-the-2020-world-cs.tsv")
ranking_qs = read_tsv("data/ranking-qs-2020-world-cs.tsv")
ranking_shanghai = read_tsv("data/ranking-shanghai-2019-world-cs.tsv")
```

## Preview Rankings

### QS Ranking


```python
def preview(ranking: List, line_cnt: int = 10) -> None:
    print(f"Length: {len(ranking)}")

    for i in range(line_cnt):
        rank, uni = ranking[i]

        print(f"{rank}\t{uni}")

preview(ranking_qs)
```

    Length: 50
    1	Massachusetts Institute of Technology (MIT)
    2	Stanford University
    3	Carnegie Mellon University
    4	University of California, Berkeley
    5	University of Oxford
    6	University of Cambridge
    7	Harvard University
    8	Ecole Polytechnique Fédérale de Lausanne (EPFL)
    9	ETH Zurich
    10	University of Toronto


### THE Ranking


```python
preview(ranking_the)
```

    Length: 50
    1	University of Oxford
    2	Stanford University
    3	ETH Zurich
    4	Massachusetts Institute of Technology (MIT)
    5	University of Cambridge
    6	Carnegie Mellon University
    7	Imperial College London
    8	Harvard University
    9	Princeton University
    10	California Institute of Technology (Caltech)


### Shanghai Ranking


```python
preview(ranking_shanghai)
```

    Length: 50
    1	Massachusetts Institute of Technology (MIT)
    2	Stanford University
    3	University of California, Berkeley
    4	Carnegie Mellon University
    5	ETH Zurich
    6	Harvard University
    7	Tsinghua University
    8	University of California, Los Angeles (UCLA)
    9	Princeton University
    10	University of Oxford


## Clean Rankings


```python
def get_unis(ranking: List) -> set:
    unis = set()
    for _, uni in ranking:
        unis.add(uni)
    
    return unis

universities_the = get_unis(ranking_the)
universities_qs = get_unis(ranking_qs)
universities_shanghai = get_unis(ranking_shanghai)
```


```python
# Check sizes
print(len(universities_the))
print(len(universities_qs))
print(len(universities_shanghai))
```

    50
    50
    50



```python
universities_all = universities_the | universities_qs | universities_shanghai  # To eliminate duplicates

# The file saved below was used to make duplicates more apparent. These were then
# eliminated by harmonizing the univerisity names across the three starting rankings.
with open("data/universities_sorted.tsv", "w") as file:
    file.write(f"{len(universities_all)}\n")
    for uni in sorted(universities_all):
        file.write(f"{uni}\n")
```


```python
len(universities_all)
```




    71



## Merge Rankings


```python
rankings_per_university = {}

rankings_cleaned = [ranking_the, ranking_qs, ranking_shanghai]

for ranking in rankings_cleaned:
    for rank, uni in ranking:
        rank = int(rank)

        if uni in rankings_per_university:
            rankings_per_university[uni].append(rank)
        else:
            rankings_per_university[uni] = [rank]
```


```python
print(rankings_per_university)  # Preview
```

    {'University of Oxford': [1, 5, 10], 'Stanford University': [2, 2, 2], 'ETH Zurich': [3, 9, 5], 'Massachusetts Institute of Technology (MIT)': [4, 1, 1], 'University of Cambridge': [5, 6, 27], 'Carnegie Mellon University': [6, 3, 4], 'Imperial College London': [7, 14, 40], 'Harvard University': [8, 7, 6], 'Princeton University': [9, 11, 9], 'California Institute of Technology (Caltech)': [10, 29], 'National University of Singapore': [11, 12, 16], 'University of California, Los Angeles (UCLA)': [12, 15, 8], 'Nanyang Technological University (NTU)': [13, 16, 13], 'Cornell University': [14, 19, 14], 'Tsinghua University': [15, 13, 7], 'Georgia Institute of Technology': [16, 27, 18], 'Hong Kong University of Science and Technology': [17, 26], 'Technical University of Munich': [18, 36], 'University College London (UCL)': [19, 17, 19], 'Ecole Polytechnique Fédérale de Lausanne (EPFL)': [20, 8, 33], 'Columbia University': [21, 19, 22], 'University of Michigan-Ann Arbor': [22, 48, 20], 'University of Toronto': [23, 10, 12], 'University of Edinburgh': [24, 23, 21], 'University of Texas at Austin': [25, 31, 11], 'University of Washington': [26, 18, 26], 'Peking University': [27, 19, 41], 'Yale University': [28, 46], 'University of Illinois at Urbana-Champaign': [29, 33, 24], 'Johns Hopkins University': [30], 'University of Montreal': [31], 'University of Pennsylvania': [31, 35], 'New York University (NYU)': [33, 19, 37], 'Université Paris Sciences et Lettres (PSL)': [34, 39], 'University of California, San Diego': [35, 36], 'Chinese University of Hong Kong (CUHK)': [36, 30, 23], 'University of Southern California': [37, 44, 15], 'University of Hong Kong': [38, 38], 'University of Chicago': [39, 47], 'University of Waterloo': [40, 24, 44], 'Zhejiang University': [41, 31], 'University of Tokyo': [42, 28], 'University of British Columbia': [43, 25, 45], 'Korea Advanced Institute of Science and Technology (KAIST)': [44, 36], 'Shanghai Jiao Tong University': [45, 34, 27], 'RWTH Aachen University': [46], 'Delft University of Technology': [47], 'Seoul National University': [47, 48], 'University of Maryland, College Park': [49, 50, 38], 'Karlsruhe Institute of Technology (KIT)': [50], 'University of California, Berkeley': [4, 3], 'University of Melbourne': [32], 'Politecnico di Milano': [40], 'Australian National University': [41, 46], 'University of Sydney': [42], 'KTH Royal Institute of Technology': [43], 'University of Amsterdam': [45], 'University of North Carolina at Chapel Hill': [17], 'University of Copenhagen': [25], 'University of Technology Sydney': [29], 'University of Wisconsin-Madison': [30], 'Weizmann Institute of Science': [32], 'University of Science and Technology of China': [34], 'Harbin Institute of Technology': [35], 'Huazhong University of Science and Technology': [39], 'City University of Hong Kong': [41], 'University of Adelaide': [43], 'Aalto University': [47], 'University of Electronic Science and Technology of China': [48], 'University of Oslo': [49], 'Hong Kong Polytechnic University': [50]}



```python
ranking_2020_world_cs = {}

for uni, ranks in rankings_per_university.items():
    while (len(ranks) < 3):
        ranks.append(100)  # If not present, we default to a #100 ranking.
    
    rank = sum(ranks) / len(ranks)

    ranking_2020_world_cs[uni] = rank 
```

## Save Final Ranking


```python
def save_ranking(ranking: dict) -> None:
    cnt = 0
    with open("data/ranking_2020_world_cs.tsv", "w") as file:
        for uni, rank in sorted(ranking.items(), key=lambda item: (item[1], item[0])):
            cnt += 1
            line = f"{cnt} \t {uni} \t {rank:.2f}"
            file.write(f"{line}\n")
            if cnt <= 10:
                print(line)  # Let's print the top 10 universities

save_ranking(ranking_2020_world_cs)
```

    1 	 Massachusetts Institute of Technology (MIT) 	 2.00
    2 	 Stanford University 	 2.00
    3 	 Carnegie Mellon University 	 4.33
    4 	 University of Oxford 	 5.33
    5 	 ETH Zurich 	 5.67
    6 	 Harvard University 	 7.00
    7 	 Princeton University 	 9.67
    8 	 Tsinghua University 	 11.67
    9 	 University of California, Los Angeles (UCLA) 	 11.67
    10 	 University of Cambridge 	 12.67


# Export Report


```python
!tree
```

    [01;34m.[00m
    ├── README.md
    ├── [01;34mdata[00m
    │   ├── ranking-qs-2020-world-cs.tsv
    │   ├── ranking-shanghai-2019-world-cs.tsv
    │   ├── ranking-the-2020-world-cs.tsv
    │   ├── ranking_2020_world_cs.tsv
    │   └── universities_sorted.tsv
    ├── environment.yml
    ├── [01;34mimages[00m
    │   └── top-ten.png
    ├── ranking_universities_2020_world_cs.ipynb
    └── [01;34mreports[00m
        ├── ranking_universities_2020_world_cs.ipynb.md
        └── ranking_universities_2020_world_cs.md
    
    3 directories, 11 files



```python
!mkdir reports
```

    mkdir: reports: File exists



```python
!jupyter nbconvert --to markdown ranking_universities_2020_world_cs.ipynb --output-dir='reports' --output='ranking_universities_2020_world_cs.md'
```

    [NbConvertApp] Converting notebook ranking_universities_2020_world_cs.ipynb to markdown
    [NbConvertApp] Writing 9439 bytes to reports/ranking_universities_2020_world_cs.md


# MOOCs

## Review in ClassCentral DataBase

 - https://www.classcentral.com/providers
 - https://www.classcentral.com/universities
 - https://www.classcentral.com/report/mooc-stats-2019/

# Ref: 
 - https://www.classcentral.com/report/cs-online-courses/
