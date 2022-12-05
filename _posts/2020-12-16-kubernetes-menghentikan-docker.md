---
layout: post
title: Kubernetes Menghentikan Docker
categories: Computer
---

Saya belum sempat menggunakan Kubernetes dan Docker secara nyata (hanya coba-coba di beberapa penyedia IaaS), eh ada kabar kalau Kubernetes telah menghentikan Docker. Apa saja sih perubahannya, siapa saja yang bakal terdampak dari pengehentian ini, jikalau saat ini telah menggunakan Docker pada Kubernetes bagaimana cara untuk migrasi dan perlukah panik terhadap kabar ini?

Seperti yang telah kita dengar, saat ini Kubernetes menghentikan Docker sebagai *runtime* mulai versi 1.20 dan mendukung *runtime* yang menggunakan CRI (Container Runtime Interface), contohnya **containerd** dan **CRI-O**

Anda tidak perlu panik, penghentian ini tidak serta merta. Pada versi 1.20 akan mendapatkan peringatan bahwa Kubernetes menghentikan Docker. Anda masih mempunyai waktu satu tahun untuk membuat rencana migrasi, karena Docker akan tidak didukung sepenuhnya pada versi 1.22 yang akan dirilis pada akhir tahun 2021.

Meskipun begitu, ada pilihan untuk tidak meng-*update* ke versi 1.22 sampai dengan ada memiliki kesiapan, disamping itu Kubernetes tetap akan memberikan *update* terkait dengan keamanan pada versi sebelumnya.

Baiklah, kita akan mencoba untuk menguraikannya satu persatu.

#### Apa saja sih yang berubah

Yang berubah hanyalah yang terjadi pada *runtime* Kubernetes. Jika menggunakan Docker untuk menmbuat *image* dan menguji di CI/CD Pipeline selama pengembangan, Docker tetap dapat digunakan. Docker menggunakan **containerd** secara internal untuk menjalankan container Docker.

Docker menggunakan **containerd** dan **containerd** ialah *runtime* containerd lain. Jadi Docker bukan hanya container *runtime* - container *runtime* hanya membentuk intinya dan Docker telah memindahkannya ke **containerd**. Docker juga menyediakan *user experience* yang memudahkan pengembang untu berinteraksi dengan mudah.

Namun ini sulit bagi Kubernetes, Kubernetes tidak dapat langsung berinteraksi dengan Docker karena tidak kompatibel dengan Kubernetes CRI. Sebagai gantinya Kubernetes menambahkan **dockershim**, ini semakin menambah kompleksitas pada container *runtime* yang sudah sangat kompleks.

Itu menjadi berlebihan, oleh karena itu Kubernetes memutuskan untuk menghentikan dukungannya karena Docker pada akhirnya juga berinteraksi dengan **containerd**, dimana dapat berinteraksi dengan Kubernetes secara langsung. Kubernetes tidak peduli dengan UI mewah sekalipun, karena menurutnya itu hanyalah sebuah mesin.

#### Siapa saja sih yang terdampak?

Jadi, apakah kita akan berhenti menggunakan Docker?Jawabannya tidak. Docker mempunyai fungsi yang berbeda. Selain cebagai container *runtime*, Docker juga mesin container yang disukai pengembang. Jika menggunakan Docker untuk membuat *image* dan CI/CD Pipeline, anda dapat terus menggunakannya.

Docker membuat container *image* dengan standar OCI, itu berarti *image* Docker dapat berjalan secara baik dengan container *runtime* apapun yang sesuai dengan OCI - ini mencakup **containerd** dan **CRI-O**

Mari kita lihat lapisan orkestrasi. Jika menggunakan Docker sebagai container *runtime* dalam kluster Kubernetes maka akan terpengaruh. Sebagai gantinya harus menggunakan *runtime* yang didukung (**containerd** dan **CRI-O**). Docker container Docker berjalan dengan baik pada kedua *runtime* tersebut.

Jika anda menggunakan layanan *managed* seperti GKE, EKS atau AKS, perlu mengecek pengaturan kluster tersebut menggunakan container *runtime* yang mana. Jika masih menggunakan Docker, perlu bekerja sama dengan penyedia cloud untuk memastikan mendapatkan upgrade yang tepat dan teruji ke *runtime* yang didukung. Penyedia cloud akan segera menyediakan saran dan jalur migrasi.

Jika telah mempunyai kluster lokal, ini akan akan membutuhkan waktu dan mungkin akan berakibat *downtime* untuk sementara waktu saat mengganti container *runtime* dan memulainya lagi dengan memodifikasi file `crictl.yaml` agar mengarah ke container *runtime* yang baru atau dengan mengizinkan `kubeadm` mendeteksi *runtime* baru secara otomatis setelah mengonfigurasi. Jika tidak ingin adanya *downtime*, ada dapat menggunakan Canary, dimana mengalihkan kluster duplikasi dengan *runtime* yang didukung dan memindahkan beban kerja ke sana kemudian mengalihkan kembali kluster yang sudah ada.

#### Kira-kira beban kerja container kena dampak gak ya?

Lebih baik jika membuat hitung-hitungan sebelum mencoba bermigrasi ke *runtime* yang baru. Jika memiliki CI/CD Pipeline yang berjalan di Kubernetes serta menggunakan pendekatan **Docker-in-Docker** dengan memasang file socket `/var/run/docker.sock` maka akan terpengaruh.

Jika tidak akan menjalankan Docker di kluster Kubernetes maka tidak pelu mengganti. Salah satu solusi terbaik saat ini adalah Kaniko, karena tidak bergantung pada Docker daemon untuk membangung container *image*.

#### Rangkuman

Meskipun Kubernetes telah membuat keputusan untuk menghentikan Docker karena lebih mendukung container *runtime* yang didukung CRI, tidak ada alasan untuk panik dengan kabar tersebut. Hal ini lantas tidak membuat Docker mati tetapi dampaknya hanya terdapi pada container *runtime* di kluster Kubernetes. Anda dapat terus mengggunakan Docker pada aktivitas pengembangan seperti biasanya.
