package script 中添加postinstall来做构建动作，这样再icode里面就可以完成构建，不再需要再提交nestBuild了
postinstall: nest build

