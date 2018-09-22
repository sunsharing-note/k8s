**master**: 默认情况下master是不可被调度的

* predicate-->priority-->select 即预选、优选、选择
* nodeName pod只运行在该node上
* nodeAffinity：功能更丰富的Node选择器，比如支持集合操作
* podAffinity：调度到满足条件的Pod所在的Node上
* 可以给节点打污点，如果pod不能容忍该污点，则不会被调度
* taint针对node,toleration针对pod

NoSchedule: 仅影响调度过程，对现存的pod对象不影响

NoExecute: 既影响调度过程，也影响现在的pod对象；不容忍的pod对象将被驱逐

PreferNoSchedule: 勉强