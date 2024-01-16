💁‍♂️Ресурсы для подготовки
========================
* [mahdibouaziz/Cloud-native-docs/killer.sh.md](https://github.com/mahdibouaziz/Cloud-native-docs/blob/2e5c4b2c6e4bbeb66511924e8b6634edd4aa2eb0/kubernetes/killer.sh.md)
* [e00049/kubernetes/cka-exam-dumps.txt](https://github.com/e00049/kubernetes/blob/16df5415d2bd6faa3a78e11233a95f905707369a/cka-exam-dumps.txt#L)
* [deemoprobe/kubernetes](https://github.com/deemoprobe/kubernetes/blob/main/Kubernetes%E9%85%8D%E7%BD%AE%E6%A1%88%E4%BE%8B.md)
* ✏[David-VTUK/CKA-StudyGuide](https://github.com/David-VTUK/CKA-StudyGuide): Guide is based on the Certified Kubernetes Administrator Exam Curriculum 1.26. Revision Topics and Lab Guide With Example Answers.

* ✅[alifiroozi80/CKA/CKA](https://github.com/alifiroozi80/CKA/blob/main/CKA/README.md): Вся теория в кратком изложении.

Commands
========
```shell
kubeadm config print init-defaults
```
```shell
kubectl api-resources --namespaced=false
```
```shell
kubectl -n rbac-test --as=system:serviceaccount:rbac-test:rbac-test-sa auth can-i get pods
```
