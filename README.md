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

### 2021-08-13

Ini berdasarkan data yang digunakan oleh riset Amany dengan properties:
- 2352 kalimat training, 296 kalimat dev, dan 306 kalimat test
- Terdiri dari kalimat sederhana
- Ditambahkan beberapa kalimat yang merupakan paraphrase (kalimat berbeda dengan AMR yang sama)
- Ditambahkan kalimat konjungsi (dua kalimat yang digabungkan dengan kata konjungsi)

#### Issues pada versi ini

- modifier yang berupa angka tidak memiliki variable, sehingga tidak sepenuhnya kompatibel dengan AMR Codec. Opsinya manual mengubah codec AMR dengan regex yang tepat, atau bisa juga dengan mengubah dataset dengan memberikan variable untuk semua instans, atau bahkan menggunakan konvensi beberapa relasi yang tanpa variable. Sehingga pada saat processing akan diubah ke kategori tertentu. Misal yang diubah adalah:
```
# ::id 1221
# ::snt Ayah mengajar di Sekolah Dasar negeri Tamanan 01 tahun ini atau Adik bermain boneka Barbie
(a / atau
   :op1 (a2 / ajar
            :ARG0 (a3 / ayah)
            :location (s / sekolah
                         :mod (d / dasar
                                 :mod (n / negeri))
                         :name (t / tamanan
-                                  :mod (_ / 1)))
+                                  :mod "1"))
            :time (t2 / tahun))
   :op2 (m / main
           :ARG0 (a4 / adik)
           :ARG1 (b / boneka
                    :name (b2 / barbie))))
```