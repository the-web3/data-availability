Eigelayer 项目流程梳理
1. 节点注册流程
- RegistryPermission----->addOperatorRegisterPermission：给节点进行节点授权注册
- EigenLayrDelegation---> registerAsOperator:  注册成为节点并能够接受质押
  - _delegate: 授权 operator 能够接受质押
- BLSPublicKeyCompendium--->registerBLSPublicKey: 注册 BLS 公钥用于验证签名
- BLSRegistry--->registerOperator: 将节点的类别，socket 链接等信息注册到合约，这个动作会发出注册事件， 同时 graph-node 可以监听到这些事件，disperser 会去获取这些信息检测，质押等信息，然后通过 socket 链接的 rpc 推送数据给节点，disperser 收集节点的承诺签名之后，将承诺签名推到合约上。

2. Staker 质押和取回质押流程
- RegistryPermission------>addDelegatorPermission：给staker 授予 stake 的权限
- InvestmentManager----->depositIntoStrategy: staker 打入要质押的金额
- InvestmentManager----->delegateTo: 将质押股份 delegate 给对应的 staker
- InvestmentManager----->setDelegatorCanWithdraw: 给 staker 授权可以取回质押
- InvestmentManager----->delegatorWithdraw: staker 取回质押

3. 节点退出流程
- RegistryPermission----->addOperatorDeregisterPermission：给节点退出授权
- BLSRegistry-------->deregisterOperator: 节点退出
- BLSRegistry----->forceDeregisterOperator: 节点强制退出

4. Rollup 数据流程
- BVM_EigenDataLayrChain: storeData----->DataLayrServiceManager:nitDataStore: 初始化要提交的数据
- BVM_EigenDataLayrChain: confirmData----->DataLayrServiceManager:confirmDataStore 确认要存储的数据

5. 奖励惩罚流程
- 