# Data AMR Bahasa Indonesia V2

## Using Utilities and Notebook
Notebooks dan utils digunakan untuk melakukan eksplorasi terutama untuk melakukan re-anotasi terhadap named-entity, dengan bantuan NER dan POS tags (NNP)

## Contributors:

- Masayu
- Verena 
- Adylan
- Amani
- Furqon
- Aditya
- Nada

## History
### 2021-10-18

#### Reanotasi untuk mempermudah rekategorisasi, terutama fokus pada named-entity :
```diff
# ::snt Bunga ditanam Bu Ani
(t / tanam
-       :ARG0 (b1 / bu
-               :name (a / ani))
+       :ARG0 (o / orang
+               :name (n / name :op1 "Bu" :op2 "Ani"))
        :ARG1 (b / bunga))
```
Secara programatik ini dilakukan dengan menggunakan regex berikut (belum handle nama dnegan satu kata saja):
- ```regex
  \([a-z0-9\s]*/ ([a-z0-9]*)[\n\s]*:name \([a-z0-9\s]*/ ([a-z0-9]*)\)\)
  ```
  dan replace dengan:
  ```
  (o / orang :name (n / name :op1 "$1" :op2 "$2"))
  ```
- Mencari kata dengan huruf besar, dan menggantinya menjadi named entity
TODO: Bu Zee Zee atau nama yang lebih dari 2 kata belum dihandle
TODO: nama jabatan blm dihandle dengan baik (pres, prof, sus, etc.) harusnya bisa spesifik, ga orang aja, mis.
```
:ARG0 (d / doctor :wiki - :name (n2 / name :op1 "Talcott"))
```
tapi kalau pak/bu/tante mah tetep aja kayak biasa
```
:name (m / name :op1 "Mr." :op2 "Vitulli")
```

#### Menghilangkan modifier redundan
Terutama partikel `ke`, 'itu' dan `di`
```diff
(b1 / berangkat
      :ARG0 (o / orang
            :name (n / name :op1 "Om" :op2 "Andi"))
+     :ARG1 (s / sekolah)
-     :ARG1 (s / sekolah
-           :mod (k / ke))        
      :time (b2 / besok))
```

#### Menambahkan Date Entity
dan beberapa modifiernya (konsider untuk tidak menggunakan modifier kompleks)
```diff
(p / pergi
      :ARG0 (k1 / kami
            :mod (k2 / keluarga))
      :ARG1 (t / tamasya)
+     :time (d / date-entity
+           :weekday (m / minggu)))
-     :time (h / hari
-           :mod (m / minggu)))
```

#### Handle Location Entity
Menjadi location (jenis location) named op
#### Handle Quantity Entity

#### Renumber nodes
Karena programmatic, pastiin nodenya juga direnumber dengan sesuai

### 2021-08-13

Ini berdasarkan data yang digunakan oleh riset Amany dengan properties:
- 2352 kalimat training, 296 kalimat dev, dan 306 kalimat test
- Terdiri dari kalimat sederhana
- Ditambahkan beberapa kalimat yang merupakan paraphrase (kalimat berbeda dengan AMR yang sama)
- Ditambahkan kalimat konjungsi (dua kalimat yang digabungkan dengan kata konjungsi)

#### Issues pada versi ini

- modifier yang berupa angka tidak memiliki variable, sehingga tidak sepenuhnya kompatibel dengan AMR Codec. Opsinya manual mengubah codec AMR dengan regex yang tepat, atau bisa juga dengan mengubah dataset dengan memberikan variable untuk semua instans, atau bahkan menggunakan konvensi beberapa relasi yang tanpa variable. Sehingga pada saat processing akan diubah ke kategori tertentu. Misal yang diubah adalah:
```diff
# ::id 1221
# ::snt Ayah mengajar di Sekolah Dasar negeri Tamanan 01 tahun ini atau Adik bermain boneka Barbie
(a / atau
   :op1 (a2 / ajar
            :ARG0 (a3 / ayah)
            :location (s / sekolah
                         :mod (d / dasar
                                 :mod (n / negeri))
                         :name (t / tamanan
-                                :mod (_ / 1)))
+                                :mod "1"))
            :time (t2 / tahun))
   :op2 (m / main
           :ARG0 (a4 / adik)
           :ARG1 (b / boneka
                    :name (b2 / barbie))))
```