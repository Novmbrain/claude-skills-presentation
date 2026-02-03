我现在presentation 的一开始就直接展示 两个例子


 BOS（Business Object Server） Onboarding Case

 首先展示高质量doc，然后展示下列workflow


1. 进入BOS 自动生成 copilot-instructions.md
2. 进入想要查看的具体class类。使用调用agent + Onboarding skill 生成具体 fucntionality 的onboarding doc。阅读doc 并和实际的代码库进行对比验证理解直到有足够的理解深度。同时将生成和确认过的onboarding doc 存入知识库
3. 引用知识库，结合具体的问题 和 业务场景 进行解决方案planning，review plan 并迭代
4. 实行TDD 根据plan 首先生成测试用例。review 测试 并迭代
5. 根据plan 进行任务划分，生成代码。
6. 使用 Java Code review skill  代码 运行测试 通过 并迭代。使用Refactoring Agent 进行代码优化
7. PR review & merge

成功案例 -> https://mxjira.murex.com/browse/MXDEF-23179





2. 个人工作流