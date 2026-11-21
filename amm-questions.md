# Raydium CP-Swap Checklist

1. 一共有哪些对外指令？每条指令谁能调用？
2. PoolState 里有哪些字段？每个字段在哪些指令里被修改？
3. AmmConfig 有什么作用？一个池子绑定一个 config 还是可以换？
4. 池子的 vault 是什么账户？owner 是谁？由哪个 PDA 签名转出？
5. 池子 PDA 的 seeds 是什么？同一对 token 能不能开多个池子？
6. token_0 和 token_1 的顺序是怎么规定的？如果传反了会怎样？
7. token_0 和 token_1 能不能是同一个 mint？
8. 能不能把别的池子的 vault 传进来？哪条约束阻止了它？
9. LP mint 的 mint authority 是谁？除了 deposit 还有哪里能铸 LP？
10. 首个 LP 的数量怎么算？为什么用这个公式？
11. 有没有锁定一部分初始流动性？锁在哪里，锁多少？
12. 捐赠攻击：先存极少量，再直接往 vault 转一大笔，后来存入的人会怎样？
13. 开池需要付费吗？费用付给谁？能不能绕过？
14. initialize、initialize_v2、initialize_with_permission 有什么区别？
15. 开池时的 open_time 有什么作用？开放前能 swap 吗？能 deposit 吗？
16. 开池时初始价格由谁决定？能不能用离谱的价格开池？
17. deposit 时用户需要存入的两种 token 数量怎么算？舍入方向是什么？
18. deposit 时铸出的 LP 数量怎么算？舍入方向是什么？
19. deposit 的滑点保护参数是什么？
20. 存入极小数量时，能不能拿到 LP 却几乎不付 token？
21. withdraw 时取出的 token 数量怎么算？舍入方向是什么？
22. withdraw 的滑点保护参数是什么？
23. 能不能把 LP 全部 withdraw 掉，让池子变空？空了之后再 deposit 会怎样？
24. 反复 deposit 再 withdraw，能不能靠舍入误差赚钱？
25. 手续费为什么不会被 LP 在 withdraw 时一起取走？
26. vault_amount_without_fee 是怎么计算的？哪些地方调用了它？
27. 储备值读的是 vault 实际余额还是内部记账？两种方式各有什么风险？
28. 有人直接往 vault 转 token（不走指令），价格会变吗？这些钱最后归谁？
29. swap_base_input 的输出怎么算？舍入方向是什么？为什么？
30. swap_base_output 的输入怎么算？舍入方向是什么？为什么？
31. 手续费从输入扣还是从输出扣？两种方式有什么区别？
32. swap 之后有没有检查 k 不减？检查在哪？
33. 去掉手续费后，k 会不会因为舍入而减少？
34. 输入极小数量时，能不能换出非零的输出？
35. 输入极小数量时，能不能逃掉手续费（手续费舍入成 0）？
36. 能不能用 amount_in = 0 调用 swap？会发生什么？
37. 能不能把池子里某一边的 token 全部换空？
38. swap 的滑点保护是怎么实现的？用户不设置会怎样？
39. 三明治攻击在这个池子上怎么实施？有什么防护？
40. trade fee 怎么拆分给 LP、protocol、fund、creator？
41. 各部分手续费分别累计在哪个字段？
42. collect_protocol_fee 谁能调用？钱能转到任意地址吗？
43. collect_fund_fee 谁能调用？钱能转到任意地址吗？
44. collect_creator_fee 和 collect_creator_fee_permissionless 有什么区别？
45. permissionless 版本任何人都能调，钱最后去哪？会不会被别人领走？
46. creator fee share 是什么？谁能创建和关闭？
47. 领取手续费后字段有没有清零？能不能重复领？
48. creator fee 是按输入 token 还是输出 token 收的？有什么区别？
49. 手续费率的上限是多少？能不能被设成 100%？
50. 改了 config 的费率，已经存在的池子会立刻受影响吗？
51. 计算过程中哪些地方用了 u128？
52. 哪些地方用的是 checked 运算，哪些是直接 + - * /？
53. 有没有 as u64 强转？会不会截断？
54. 两种 token 的 decimals 不同时，计算会出问题吗？
55. 极大数额（接近 u64 上限）时，计算会不会溢出？
56. 支持 Token-2022 吗？哪些扩展被允许，哪些被拒绝？
57. Token-2022 带 transfer fee 的 token，池子按什么数量记账？
58. transfer fee 发生在 swap 的输入侧和输出侧时，分别怎么处理？
59. 用户的滑点参数是按扣 transfer fee 之前还是之后的数量比较？
60. 带 transfer hook 的 token 能不能进池子？如果能，有什么风险？
61. 带 permanent delegate 的 token 有什么风险？程序有没有拦截？
62. 带冻结权限的 token：mint 方冻结 vault 会怎样？
63. support_mint_associated 是什么？谁能添加，谁能删除？
64. 程序怎么区分传入的是 Token 还是 Token-2022 程序？能不能传错？
65. oracle 记录了什么？在哪些指令里更新？
66. oracle 能不能在一个区块内被操纵？外部协议直接用它安全吗？
67. 池子有哪些状态位？分别禁止什么操作？
68. 池子被暂停时，用户还能做什么？能撤出资金吗？
69. update_pool_status 谁能调用？
70. admin 是怎么确定的？硬编码还是存在链上？
71. admin 能不能直接动用户的钱？
72. admin 能做的最坏的事是什么？
73. permission PDA 是什么？谁能创建，有什么用？
74. collect_excess_lamports 是干什么的？会不会把账户的租金也收走？
75. 哪些账户可以被关闭？关闭后 lamports 去哪？能不能被重新初始化？
76. init_if_needed 用在哪？会不会被别人抢先创建账户？
77. 用户的 token 账户需要校验 owner 吗？传入别人的账户会怎样？
78. 每条指令有没有检查 signer？哪些账户必须签名？
79. 同一个账户能不能在一条指令里传两次（比如 input_vault 和 output_vault 相同）？
80. 同一笔交易里先 swap 操纵价格，再 deposit，然后反向 swap，能获利吗？
81. 先 swap，再 collect fee，再 withdraw，状态会不会不一致？
82. 闪电贷放大规模后，以上哪些攻击会变得有利可图？
83. 事件（event）里记录的数据和实际状态一致吗？链下依赖事件会不会被误导？
84. 每条指令执行前后，池子总资产、LP 总份额、各类应付手续费三者的关系是否保持一致？
85. 程序的升级权限归谁？这对用户意味着什么？
86. 编译时的 feature（devnet/localnet）如果在主网部署时开错了，会发生什么？
88. creator fee 的 share_rate 是在 swap 时确定，还是在领取时确定？
89. 创建者领取前，admin 把 creator_fee_share 的 share_rate 设成 1_000_000，已累计的 creator fee 会全部归 protocol 吗？
90. 调用 collect_creator_fee_permissionless 的人，能不能让创建者少拿到钱？
91. 调用者能不能通过传入不同的 creator_fee_share 账户，让程序用 amm_config 的默认 share_rate 代替自定义的 share_rate？
92. 同一个创建者在两个不同 amm_config 下各开一个池子，只为其中一个 config 创建了 creator_fee_share，另一个池子领取时用的是哪个 share_rate？
93. 调用 settle_creator_fee 前后，vault_amount_without_fee 的返回值会变吗？
94. creator fee 里分给 protocol 的那部分，最终通过哪条指令转出池子？
95. 创建者的 token 账户被冻结后，池子里累计的 creator fee 还能被领走吗？
96. collect_creator_fee 和 collect_creator_fee_permissionless 在账户约束上有什么不同？
97. pool_creator 是一个 PDA 时，collect_creator_fee_permissionless 能成功吗？
98. creator_fee_on 为 OnlyToken0，用户用 token_1 换 token_0 时，creator fee 从输入侧还是输出侧收取？
99. creator fee 从输出侧收取时，swap 后 vault 减少的数量等于转给用户的数量吗？
100. creator fee 从输入侧收取时，合并计算再拆分得到的 trade fee，和单独按 trade_fee_rate 计算的 trade fee，最多相差多少？
101. trade_fee_rate 和 creator_fee_rate 都设成各自允许的最大值时，swap 会发生什么？
102. swap_base_output 在 creator fee 从输入侧收取时，用户实际支付的数量包含 creator fee 吗？
103. 同样的输入金额，creator fee 从输入侧收和从输出侧收，用户拿到的输出哪个更多？
104. 用 initialize 开的池子，之后有没有办法开启 creator fee？
105. 有没有代码路径在 enable_creator_fee 为 false 时仍然收取 creator fee？
106. initialize_with_permission 开出的池子，pool_creator 是 payer 还是另一个账户？
107. 在 open_time 之前能不能 deposit？
108. 开池时 init_amount_0 和 init_amount_1 都传 1，能成功吗？
109. 在 remaining_accounts 里传入另一个 mint 的 support_mint_associated 账户，能让不在白名单里的 Token-2022 mint 开池成功吗？
110. 一个 Token-2022 mint 的 support_mint_associated 被关闭后，这个 mint 已有的池子还能 swap 吗？
111. 带 InterestBearingConfig 扩展的 mint，池子计算储备时用的是原始数量还是 UI 数量？
112. 输出 token 带 transfer fee 时，minimum_amount_out 是和扣 transfer fee 之前还是之后的数量比较？
113. swap 后的 k 检查，用的是扣除各类手续费后的储备，还是 vault 实际余额？
114. oracle 的价格是在 swap 执行前还是执行后更新？
115. recent_epoch 被用来做什么？
116. lp_supply 和 LP mint 的链上 supply，在什么情况下会不相等？
117. withdraw 计算份额时，分母用的是 lp_supply 还是 LP mint 的 supply？
118. admin 修改 trade_fee_rate 后，已存在的池子下一笔 swap 立刻按新费率收费吗？
119. admin 修改 protocol_fee_rate 后，修改前已累计的 protocol fee 会被重新计算吗？
120. 同一笔交易里先 deposit、中间有人 swap、再 withdraw，这个 LP 能分到这笔 swap 的手续费吗？
121. 先把价格推到极端，再用极小金额 deposit 后立刻 withdraw，能拿回比存入更多的价值吗？
122. 每累计 1 个单位的 creator fee 就领取一次，protocol 能分到 creator fee 吗？
123. 有没有办法让 vault 余额小于三类手续费之和？
124. 池子的 swap 被禁用时，creator fee 还能领取吗？
125. 在 creator fee 功能上线前创建的旧池子，enable_creator_fee 的值是多少？
126. protocol fee 的接收地址由谁决定？
