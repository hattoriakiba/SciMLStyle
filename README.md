# SciML Julia 风格指南

[![SciML 编码风格](https://img.shields.io/static/v1?label=code%20style&message=SciML&color=9558b2&labelColor=389826)](https://github.com/SciML/SciMLStyle) [![全局文档](https://img.shields.io/badge/docs-SciML-blue.svg)](https://docs.sciml.ai/SciMLStyle/stable/)

SciML 风格指南是适用于 Julia 编程语言的风格指南。它被 [SciML 开源科学机器学习组织](https://sciml.ai/) 所使用。因此，它在社区开放讨论。请提交一个 issue 或开启一个 PR 来讨论对该风格指南的修改。

**目录**
- [SciML Julia 风格指南](#SciML-Julia-风格指南)
  - [代码风格徽章](#代码风格徽章)
  - [SciML 风格的总体原则](#SciML-风格的总体原则)
    - [更新一致或因循守旧](#更新一致或因循守旧)
    - [社区贡献准则](#社区贡献准则)
    - [开源贡献是从小到大的历练](#开源贡献是从小到大的历练)
    - [优先使用通用化的代码](#优先使用通用化的代码)
    - [尽可能匹配用户使用的类型](#尽可能匹配用户使用的类型)
    - [尽可能使用合适的 Trait 及通用的接口](#尽可能使用合适的-Trait-及通用的接口)
    - [仅在将其作为语法糖的情况下使用宏](#仅在将其作为语法糖的情况下使用宏)
    - [应该在高层捕获错误，并提供完善后的信息](#应该在高层捕获错误，并提供完善后的信息)
    - [优先使用子包和接口包，而不是 Requires.jl](#优先使用子包和接口包，而不是-Requiresjl)
    - [函数应减少分配内存并重用缓存，或使用不可变的输入](#函数应减少分配内存并重用缓存，或使用不可变的输入)
    - [性能足够高时，首选 Out-of-Place 和不可变（Immutability）的写法](#性能足够高时，首选-Out-of-Place-和不可变（Immutability）的写法)
    - [测试应该囊括尽可能多的输入类型](#测试应该囊括尽可能多的输入类型)
    - [子模块应该适时改写为子包或独立包](#子模块应该适时改写为子包或独立包)
    - [尽可能避免使用全局作用域](#尽可能避免使用全局作用域)
    - [尽可能基于类型编程并保持类型稳定](#尽可能基于类型编程并保持类型稳定)
    - [尽可能避免使用闭包](#尽可能避免使用闭包)
    - [数值有关的功能应使用适当且通用的数值接口](#数值有关的功能应使用适当且通用的数值接口)
    - [函数意义的原则](#函数意义的原则)
    - [尽可能将编码时的考虑作为用户可用的选项](#尽可能将编码时的考虑作为用户可用的选项)
    - [尽可能重复使用代码](#尽可能重复使用代码)
    - [尽可能避免将函数遮蔽](#尽可能避免将函数遮蔽)
    - [不使用不再维护的依赖](#不使用不再维护的依赖)
  - [具体规则](#具体规则)
    - [首要规则](#首要规则)
    - [通用命名原则](#通用命名原则)
    - [注释](#注释)
    - [模块](#模块)
    - [函数](#函数)
    - [函数参数优先级](#函数参数优先级)
    - [测试与持续集成](#测试与持续集成)
    - [空格](#空格)
    - [命名元组](#命名元组)
    - [数值](#数值)
    - [三元操作符](#三元操作符)
    - [For 循环](#For-循环)
    - [函数类型标注](#函数类型标注)
    - [结构类型标注](#结构类型标注)
    - [宏](#宏)
    - [类型与类型标注](#类型与类型标注)
    - [软件包版本规范](#软件包版本规范)
    - [文档](#文档)
    - [错误处理](#错误处理)
    - [数组](#数组)
    - [行尾](#行尾)
    - [VS Code 设置](#VS-Code-设置)
    - [JuliaFormatter](#juliaformatter)
- [参考](#参考)


## 代码风格徽章

在你的项目的 `README.md` 文件中加上这个徽章，让参与你项目的同伴知道此项目遵守了 SciML 代码风格！

```md
[![SciML Code Style](https://img.shields.io/static/v1?label=code%20style&message=SciML&color=9558b2&labelColor=389826)](https://github.com/SciML/SciMLStyle)
```

## SciML 风格的总体原则

### 更新一致或因循守旧

正如 Python 的 PEP 8 所说：

> 风格指南是有关一致性的准则。代码与该风格指南保持一致性很重要，在整个项目的尺度下保持一致性更为重要，而最重要的是在一个模块或函数内保持风格的一致性。

> 此外，不可或缺的是：知晓何时打破一致性 —— 因为风格指南并非无所不能。当你陷入选择写法的困境，请仔细思考判断。参考其他案例，并采取可能最好的方案。并且，不要耻于提问！

SciML 组织中的有一些仍受支持的过时代码，由研究人员捐赠并加以维护。保持风格的一致性是首要目标，因此应该在整个代码库的水平上作出修改以符合风格指南，而不仅仅是更新一个文件。

### 社区贡献准则

你可以查看 [ColPrac](https://github.com/SciML/ColPrac) 获取完整的社区贡献指南。但需要强调的是，一个 PR 应该只做一件事。也就是，更新软件包代码风格的 PR 不应与基本代码的贡献混在一起。这种做法使得大规模的风格改进，和可能引发不定性结果的实质性代码修改，得以互不干涉。

### 开源贡献是从小到大的历练

如果评判贡献代码好坏的标准，是任何一个 PR 都需要考虑到所有人的所有想法；那么这会在维护者和新的参与者之间划出一道鸿沟。比起严格的要求，原则上只需要在一开始尽可能做到正确，随时间推移，慢慢增加代码的通用性即可。所有建议的功能都需要通过测试，并且任何已知的普遍存在问题都需要被记录在 issue 中（如果可以的话，使用 `@test_broken` 测试）。不过，比如 PR 中存在一个不与 GPU 兼容的函数，则不能因为这个函数无法通过测试而阻止合并，相反，这将激励同伴们提交下一个 PR 来改进该函数的支持。

### 优先使用通用化的代码

例如以下代码：

```julia
function f(A, B)
    for i in 1:length(A)
        A[i] = A[i] + B[i]
    end
end
```

以上写法有两个缺陷。其一，该函数假定了 `A` 使用了基于 “1” 的索引方法，而对于 [OffsetArrays](https://github.com/JuliaArrays/OffsetArrays.jl) 和 [FFTViews](https://github.com/JuliaArrays/FFTViews.jl) 的用例则会出现问题。其二，该函数需要使用索引，但并不是所有的数组类型都支持索引（例如 [CuArrays](https://github.com/JuliaGPU/CuArrays.jl)）。对此，通用且兼容性更好的实现方法是使用广播，例如：

```julia
function f(A, B)
    @. A = A + B
end
```

使用广播，将支持更多的数组类型。

### 尽可能匹配用户使用的类型

如果 `f(A)` 以某些集合作为输入，并使用这些集合的内容进行计算。那么，理想情况下，如果用户传入的 `A` 是一个 `Array` 对象，那么函数内的计算应该通过 `Array` 进行。如果 `A` 是一个 `CuArray` 对象，那么应该使用 `CuArray` 进行计算（或在不支持的情况下抛出错误）。因此，在你编写 `f` 时，使用类似 `similar(A)` 的通用方法构建数组，优于使用诸如 `Array(undef,size(A))` 这类非通用的构造函数。除非你明确这个函数在文档中已经说明了它的不通用性。

### 尽可能使用合适的 Trait 及通用的接口

Julia 提供了许多不同的接口，例如：

- [Iteration](https://docs.julialang.org/en/v1/manual/interfaces/#man-interface-iteration)
- [Indexing](https://docs.julialang.org/en/v1/manual/interfaces/#Indexing)
- [Broadcast](https://docs.julialang.org/en/v1/manual/interfaces/#man-interfaces-broadcasting)

应该尽可能使用这些接口。例如，在定义广播重载时，应按照文档的建议实现 `BroadcastStyle`，而不是通过 `copyto!` 重载来绕过广播机制。

当缺少接口函数时，应该把相关功能的函数添加到 Base Julia 中或额外创建一个接口包，就像 [ArrayInterface.jl](https://github.com/JuliaArrays/ArrayInterface.jl) 所做的那样。Traits 应该在合适的时候声明并使用： 例如，如果一行代码需要修改数组，应该先检查 trait `ArrayInterface.ismutable(A)`，如果该数组是不可变的，应该抛出完善后的错误信息（或者提供一个不要求可变的代码版本）。

举个例子：雅可比（Jacobian）矩阵的生成。在许多科研领域的应用，可能需要从用户输入的 `u0` 中生成雅可比（Jacobian）缓存。你可能天真地以为，可以用 `J = similar(u0, length(u0), length(u0))` 生成雅可比（Jacobian）矩阵。但这样生成的雅可比（Jacobian） 矩阵 J 却会是一个 Matrix 类型的矩阵。

### 仅在将其作为语法糖的情况下使用宏

宏定义了新的语法，因此它们不但使代码与其他编码风格更难组合，而且需要事先熟悉才能易于理解。所以你应该拷问自己：“阅读你代码的人能够轻松想象出它生成了什么样的代码吗？” 例如，Soss.jl 的用户可能不知道以下代码生成了什么：

```julia
@model (x, α) begin
    σ ~ Exponential()
    β ~ Normal()
    y ~ For(x) do xj
        Normal(α + β * xj, σ)
    end
    return y
end
```

因此，尽可能不要使用这种宏作为接口。不过，像 [`@muladd`](https://github.com/SciML/MuladdMacro.jl) 这样的宏在代码中很容易理解（它能[准确且高效](https://en.wikipedia.org/wiki/Multiply%E2%80%93accumulate_operation)地使用递归将 `a*b + c` 转换为 `muladd(a,b,c)`），因此可以使用这样的宏，就像这样：

```julia
julia> @macroexpand(@muladd k3 = f(t + c3 * dt, @. uprev + dt * (a031 * k1 + a032 * k2)))
:(k3 = f((muladd)(c3, dt, t), (muladd).(dt, (muladd).(a032, k2, (*).(a031, k1)), uprev)))
```

我们推荐使用上述写法。同类的宏包括：

- `@inbounds`
- [`@muladd`](https://github.com/SciML/MuladdMacro.jl)
- `@view`
- [`@named`](https://github.com/SciML/ModelingToolkit.jl)
- `@.`
- [`@..`](https://github.com/YingboMa/FastBroadcast.jl)

一些性能相关的宏，如 `@simd`、`@threads` 或者来自 LoopVectorization.jl 的 [`@turbo`](https://github.com/JuliaSIMD/LoopVectorization.jl) 是例外，它们生成的代码可能对大部分用户来说是陌生的。然而，这些写法是合理的，因为它们只是语法糖：除了性能之外，它们不会（或者不应该）以可测定的方式改变程序的行为。

### 应该在高层捕获错误，并提供完善后的信息

使用防御性编程检查潜在的错误，避免它们出现在软件包的更深层次。例如，存在一个函数 `f(u0,p)`，如果 `u0` 的大小与 `p` 不同，函数会出错。那么你应该在函数开始时就检查这个潜在问题，并适时抛出一个该作用域的错误，指出 “参数（parameters）和初始条件（initial condition）应该大小一致”。

你应该在错误信息中输出用户所知道的 API 术语，而不是指向内部的实现细节。在使用用户熟悉的语言描述清楚问题的同时，最好包含一些解决问题的建议。

### 优先使用子包和接口包，而不是 Requires.jl

尽可能的避免使用 Requires.jl。倾向于使用你需要的某个特定的接口包。例如，要定义自动微分规则，你只需要依赖 [ChainRulesCore.jl](https://github.com/JuliaDiff/ChainRulesCore.jl) 而不用依赖整个 ChainRules.jl 系统；要定义 Plots.jl 的 recipes 你只需要使用 [RecipesBase.jl](https://github.com/JuliaPlots/RecipesBase.jl) 而不用使用整个 Plots.jl。

否则，比起使用 Requires.jl 解决条件依赖，你应该创建一个子软件包；换句话说，在同一个 GitHub 仓库里保存一份具有独立版本控制和包管理的独立软件包。例如，在 [Optimization.jl](https://github.com/SciML/Optimization.jl) 中，有 [OptimizationBBO.jl](https://github.com/SciML/Optimization.jl/tree/master/lib/OptimizationBBO) 这样的子软件包，用于支持 BlackBoxOptim.jl。

以下是强烈推荐你了解的接口包：

- [ChainRulesCore.jl](https://github.com/JuliaDiff/ChainRulesCore.jl)
- [RecipesBase.jl](https://github.com/JuliaPlots/RecipesBase.jl)
- [ArrayInterface.jl](https://github.com/JuliaArrays/ArrayInterface.jl)
- [CommonSolve.jl](https://github.com/SciML/CommonSolve.jl)
- [SciMLBase.jl](https://github.com/SciML/SciMLBase.jl)

### 函数应减少分配内存并重用缓存，或使用不可变的输入

可变（Mutating）代码和不可变（Non-Mutating）代码仿佛是两个完全不同的世界。当你的代码完全不可变（Immutable）时，编译器可以更好的推断依赖关系，优化代码并检查错误。然而有些时候，充分利用可变（Mutation）的代码，甚至可以超越当今最好的编译器所生成的代码。尽管如此，当你混合两种模式的代码时，结果会变得很糟糕。这不只仅仅是一种编码风格的混合，还会导致潜在的非局部性（Non-locality）和编译器校验问题，这是因为这些代码没有充分利用好可变性。

### 性能足够高时，首选 Out-of-Place 和不可变（Immutability）的写法

可变性（Mutation）可以通过减少堆的分配（Heap Allocations）来提高性能然而，在特定情况下，如果进行堆的分配没有帮助，就不要使用可变性（Mutation）写法除非可变性可以带来立竿见影的效果。例如，在矩阵足够大的情况下，`A*B` 与 `mul!(C,A,B)` 的速度是相当的，此时，更倾向于编写 `A*B`（除非函数的其他部分严格地不进行内存分配，为了一致性就应该写作 `mul!`）。

类似的，除非改变结构体是普遍需要的，否则更倾向于使用 `struct` 而不是 `mutable struct`； 即使改变结构体的情况很普遍，你也要尽可能使用 [Setfield.jl](https://github.com/jw3126/Setfield.jl) 来实现。因为编译器会对不可变（Immutable）的结构体进行优化，所以不嫌麻烦的话，尽可能书写不可变的代码，这使得你的程序更加高效。

### 测试应该囊括尽可能多的输入类型

如果不考虑输入类型，代码覆盖率的数字就毫无意义。例如，你可以使用 `Array` 覆盖所有代码，但这不会检测 `CuArray` 的兼容性。因此，不仅是行数覆盖率，类型覆盖率也应该作为一个重要的覆盖率指标。对于数值，最好考虑以下几种类型：

- `Float64`
- `Float32`
- `Complex`
- [`Dual`](https://github.com/JuliaDiff/ForwardDiff.jl)
- `BigFloat`

对于数组则是：

- `Array`
- [`OffsetArray`](https://github.com/JuliaArrays/OffsetArrays.jl)
- [`CuArray`](https://github.com/JuliaGPU/CUDA.jl)

### 子模块应该适时改写为子包或独立包

一个软件包应该只做好一件事。如果有些代码足够独立，以至于可以单独作为一个子模块； 那么你应该考虑将它们分离出来，进行完善的测试并编写文档，以便提供给其他包使用。

### 尽可能避免使用全局作用域

应尽可能避免使用全局变量。当不得不使用全局变量时，全局变量应该是常量，并且用下划线隔开的大写字母命名（例如，`MY_CONSTANT`）并将它们的定义放在文件的开始，在导入与导出之后，在 `__init__` 函数之前。如果你真的要使用可变（Mutable）的全局操作，你应该考虑使用可变容器（Mutable Container）。

### 尽可能基于类型编程并保持类型稳定

基于类型编程并保持类型稳定，不仅可以使编译器生成高度优化的代码，同时也可以提高编译速度。你需要保证容器（Containers）具有准确的类型定义，保证函数只处理适当的参数，保证类型始终具体且明确。

### 尽可能避免使用闭包

闭包可能会使不稳定的类型问题难以被追踪和调试；长远来看，出于防御性编程的目的，你应该在一开始就尽可能避免使用闭包。纵使一些闭包可能不会引起问题，但这么做也可以节省时间。类似情景也存在于阅读带有闭包的代码的过程；如果有人在追踪类型不稳定的问题，不含闭包的代码显然更方便调试。如果你想要在外部作用域中更新变量，可以明确地使用 `Ref` 或自定义的结构体来实现。例如：
```julia
map(Base.Fix2(getindex, i), vector_of_vectors)
```
这种写法要优于以下两种：
```julia
map(v -> v[i], vector_of_vectors)
```
或
```julia
[v[i] for v in vector_of_vectors]
```

### 数值有关的功能应使用适当且通用的数值接口

虽然你可以在包内使用 `A\b` 进行线性求解，但这并不意味着你应该这样做。这个接口仅适用于执行因式分解，因此该写法降低了可扩展性、以及 `A` 的类型的兼容性。相反，在包内进行的线性求解，应该使用 LinearSolve.jl； 同样的，非线性求解应该使用 NonlinearSolve.jl； 编码优化方面应该使用 Optimization.jl； 诸如此类。这允许在不依赖于每个求解器包的情况下向用户提供完整的通用选择，其实质是在每个包内重新创建一个通用的接口。

### 函数意义的原则

一个函数只做一件事。就好比每次调用 `+` 都代表着“在指定的类型上做加法”。虽然理论上你可以为 `+` 添加不同含义的用法，但在用 `+` 表示加法的通用代码语境中，这是不合适的。为了使通用的代码运作良好，你需要考虑每个函数的意义。每次调用函数都应该是其含义的实例化。

### 尽可能将编码时的考虑作为用户可用的选项

你应该尽可能地将你在编程时考虑的多种数值和方法，作为代码的选项暴露给用户使用。这可能会带来一些意想不到的代码的重复利用。

### 尽可能重复使用代码

如果一个软件包里含有一个你需要的函数，那就使用这个软件包并将其作为一个依赖。如果该函数缺少某个功能，建议为该软件包添加这个功能，再把它作为依赖。如果这个依赖有一些潜在的问题，比如它的加载速度很慢，你应该考虑帮助该软件包解决这个问题，最后再将它作为依赖。只有你不可能帮助这个包变得“足够好用”的情况下，你才应该放弃使用它。如果你放弃了这个包，你应该考虑为你所需要的功能构建一个新的包。

### 尽可能避免将函数遮蔽

在 Julia 中，在不同命名空间里的两个函数可以使用相同的函数名。例如，`X.f` 和 `Y.f` 是两个不同的函数，具有不同的调用方式，但名称却相同。当然，这种情况应该尽可能避免。比起创建一个 `MyPackage.sort`，当你想要添加的新用法符合基本函数的原则时，你应该为你的类型将这些用法添加到 `Base.sort` 中。如果不符合，考虑使用不同的函数名称。虽然使用 `MyPackage.sort` 并不会产生冲突，但对于大多数不熟悉你的代码的人来说，这可能会令人困惑，因此考虑到一些阅读代码的新手，使用 `MyPackage.special_sort` 会更方便他人熟悉。

### 不使用不再维护的依赖

只有当包的维护者能够及时作出响应时，才应该依赖于这些包。好的代码需要好的社区。如果它的维护者在被多次提醒后，两周内仍未对问题做出任何回应，那么应该考虑移除所使用的该组织的所有依赖项。注意，有些问题可能需要远超两周的时间才能解决，重要的是维护者要保持沟通的开放性、一致性和及时性。

## 具体规则

### 首要规则

- 使用 4 个空格进行缩进，而不是使用 Tab。
- 每行的字符应在 92 个之内。

### 通用命名原则

- 类型名称应该使用 `驼峰式命名法`。
- 结构名称应该使用 `驼峰式命名法`。
- 模块名称应该使用 `驼峰式命名法`。
- 函数名称应该使用 `蛇形命名法`（全部小写）。
- 变量名称应该使用 `蛇形命名法`（全部小写）。
- 常量名称应该使用 `蛇形命名法`（全部大写）。
- 抽象类型名称应该以 `Abstract` 开头。
- 所有类型变量名称应该以唯一的大写字母开头，且根据语义化命名。
- 优先使用完整的单词而不是它的缩写。
- 用于表示包内部或私有的变量应该以两个下划线作为前缀，即 `__`。
- 在为数学实体（Mathematical Entity）命名时，可以使用单个字母，因为该实体的目的和意义只有下游调用者才知道。例如，在实现 `*(a::AbstractMatrix, b::AbstractMatrix)` 时，名称 `a` 和 `b` 都是合适的，因为这些参数的“含义”（除了作为已经由类型描述的矩阵的数学含义），只有调用者知道。
- 可以在代码中使用 Unicode 来提高可读性，但在任何情况下，都不应该在公共 API 中使用 Unicode。这是为了支持无法使用 Unicode 的终端而设定的：如果一个关键字参数必须是 η，那么在不支持 Unicode 输入的集群上使用它可能会被忽略。

### 注释

- 使用 `TODO` 来标记待办注释，使用 `XXX` 来标记对当前有问题代码的注释。
- 在注释中使用反引号（例如，`` `variable_name` ``）引用代码。
- 考虑使用代码本身来体现一些原本应该在注释里体现的信息（语义化）。例如，比起使用 `# fx applies the effects to a tree`，仅仅把函数名改为 `apply_effects(tree)` 就足够了。
- 指向 GitHub issues 和 PRs 的注释应该包含对应的 URL。只有当行内注释符合行长度限制时才可以使用行内注释。如果你的注释无法放在一行内，那么将注释放置在所涉及内容的上方：

```julia
# Yes:

# Number of nodes to predict. Again, an issue with the workflow order. Should be updated
# after data is fetched.
p = 1

# No:

p = 1  # Number of nodes to predict. Again, an issue with the workflow order. Should be
# updated after data is fetched.
```

- 一般而言，我们更倾向于将注释放置在代码行或函数之上，而不使用行内注释。

### 模块

- 模块应该在文件的开头或在一个 `module` 的定义后导入。
- 在包中，模块导入应该使用 `import` 关键字或明确声明导入的功能，例如 `using Dates: Year, Month, Week, Day, Hour, Minute, Second, Millisecond`。
- `import` 和 `using` 语句应该分开，并用一个空行分隔。

```julia
# Yes:
import A: a
import C

using B
using D: d

# No:
import A: a
using B
import C
using D: d
```

- 大量的导入项应该在行内使用逗号分隔进行导入。

```julia
# Yes:
using A, B, C, D

# No:
using A
using B
using C
using D

# No:
using A,
      B,
      C,
      D
```

- 导出的变量应该被视为公共 API 的一部分，改变它们的接口将被视为破坏性改动。
- 任何导出变量都应该是唯一的。换句话说，不要导出诸如 `f` 这样的名称，这很可能与其他代码发生冲突。
- 包含模块定义的文件不应包含任何在该模块之外运行的代码。也就是说，该模块应在文件开始使用关键字 `module` 声明，并在文件底部使用 `end` 结尾。除可能存在的模块之前的文档字符外，模块的前后不应该有其他代码。在这种情况下，模块的块内代码**不**应该缩进。
- 某些时候出于测试或为了给枚举命名空间，需要在文件的中部声明一个子模块。在这种情况下，模块的块内代码**应该**缩进。

### 函数

- 当函数适合写在一行内时使用短函数（Short-form Function）形式。

```julia
# Yes:
foo(x::Int64) = abs(x) + 3

# No:
foobar(array_data::AbstractArray{T}, item::T) where {T <: Int64} = T[
    abs(x) * abs(item) + 3 for x in array_data
]
```

- 函数应该要求输入参数，除非提供历史上广受认可的默认值，或确保默认值在 > 95% 的情况下都是合适的。例如，大多数情况下，微分方程求解器的容差默认值为 `abstol=1e-6、reltol=1e-3`，这样的参数绘制出来的图形是正确的，而且这些参数从 90 年代开始就被普遍认可。在这种情况下，使用历史认可、或最常用的值作为默认值是合理的。然而，如果想要实现 `GradientDescent`（梯度下降）算法，则学习率（Learning Rate）需要根据每个应用程序进行调整（基于梯度的大小），因此不建议使用默认值，如 `GradientDescent(learning_rate = 1)`。
- 没有默认值的参数应该被作为位置参数。需求关键字参数的较新语法可能很有用，但不应被滥用。但有一个特殊情况是接受“非此即彼”参数的情况，例如，如果定义 `g` 或 `dgdu` 的其中一个参数就足够了，那么如果无法使用不同类型的不同调度，则建议将它们都设为 `= nothing` 的关键字参数，然后检查其中哪个参数不是 `nothing`（并抛出适当的错误）。
- 在调用函数时，始终使用一个分号将关键字参数和位置参数分隔开。这样做可以避免在模棱两可的情况下出错（如对 Dict 进行集合解包（splatting））。
- 当编写一个向另一个函数发送大量关键字参数的函数时，比如向微分方程求解器发送关键字参数，应该使用命名元组关键字参数，而不是展开关键字参数。例如，使用 `diffeq_solver_kwargs = (; abstol=1e-6, reltol=1e-6,)` 作为 API，并使用 `solve(prob, alg; diffeq_solver_kwargs...)` 而不是对所有关键字参数使用集合解包。
- 修改参数的函数，其名称应该以 `!` 结尾。
- [避免类型盗用。](https://docs.julialang.org/en/v1/manual/style-guide/#Avoid-type-piracy) 也就是说，不要在不拥有的类型上向不拥有的函数添加方法。要么拥有类型，要么拥有函数。
- 函数应优先使用实例而不是类型作为参数。例如，对于求解器类型 `Tsit5`，接口应该使用 `solve(prob, Tsit5())`，而不是` solve(prob, Tsit5)`。这么做是有多方面的原因的。首先，一个类型的传递有不同的特化规则，因此，除非在使用它的分派中写入`::Type{Tsit5}`，否则它的运行速度可能会比较慢。其次，这样做可以利用默认参数和关键字参数来扩展选择，这可能对以后的某些类型有用。使用这种格式可以持续不断为方式添加更多选项。
- 如果参数数量太多，在一行内超过了 92 个字符，那么尽可能在一行内使用尽可能多的参数，并在每一行的开头保持相同的缩进，最好与 `(` 处的列对齐，但如果函数名特别长，则可以向左移动。例如：

```julia
# Yes
function my_large_function(argument1, argument2,
                           argument3, argument4,
                           argument5, x, y, z)

# No
function my_large_function(argument1,
                           argument2,
                           argument3,
                           argument4,
                           argument5,
                           x,
                           y,
                           z)
```


### 函数参数优先级

1. **函数参数**。将函数参数放在首位，允许使用 [`do`](https://docs.julialang.org/en/v1/base/base/#do) 块来传递多行匿名函数。

2. **I/O 流**。指定 `IO` 对象允许首先将函数传递给 [`sprint`](https://docs.julialang.org/en/v1/base/io-network/#Base.sprint) 等函数，例如 `sprint(show, x)`。

3. **将被修改的输入**。例如，在 [`fill!(x, v)`](https://docs.julialang.org/en/v1/base/arrays/#Base.fill!) 中，`x` 是被修改的对象，而且在要插入到 `x` 中的值之前出现。

4. **类型**。传入何种类型通常意味着函数将输出何种类型。在 [`parse(Int, "1")`](https://docs.julialang.org/en/v1/base/numbers/#Base.parse) 中，类型应位于要解析的字符串之前。类型在出现最前方的例子有很多，不过需要注意的是，在 [`read(io, String)`](https://docs.julialang.org/en/v1/base/io-network/#Base.read) 中，`IO` 参数出现在类型之前，这与本段概述的优先级顺序一致。

5. **不将被修改的输入**。在 `fill!(x, v)` 中，`v` 在 `x` 之后，并且 <0>v</0> 是 *不* 被改变的。

6. **Key**。对于关联集合，这是键值对的键。对于其他索引集合，这是索引。

7. **Value**。对于关联集合，这是键值对的值。在类似 [`fill!(x, v)`](https://docs.julialang.org/en/v1/base/arrays/#Base.fill!) 这样的情况下，这是 `v`。

8. **其他一切**。任何其他参数。

9. **可变参数**。这是指可以在函数调用的末尾无限列举的参数。例如，在 `Matrix{T}(undef, dims)` 中，维度可以作为一个[`元组`](https://docs.julialang.org/en/v1/base/base/#Core.Tuple)存在，例如 `Matrix{T}(undef, (1,2))`，或者作为一个[`变参（元组）`](https://docs.julialang.org/en/v1/base/base/#Core.Vararg)存在，例如 `Matrix{T}(undef, 1, 2)`。

10. **关键字参数**。在Julia中，关键字参数无论如何都必须在函数定义的最后；它们在这里列出是为了完整起见。

绝大多数函数不会使用上述列出的所有种类的参数；数字只是表示应该用于函数的任何适用参数的优先级。

### 测试与持续集成

- 高层级的 `runtests.jl` 文件只应该用于切换到其他不同测试文件。
- 每一组测试都应该包含在[`@safetestset`](https://github.com/YingboMa/SafeTestsets.jl)中。标准的`@testset`不能完全包含所有定义的值，例如在`@testset`中定义的函数，可能会因此导致"泄漏"。
- 测试用的 include 应该写在一行中，例如：

```julia
@time @safetestset "Jacobian Tests" include("interface/jacobian_tests.jl")
```

- 每个测试脚本都应该是可以独立运行并且结果完全可复现的。也就是说，能够复制该脚本并得到相同结果。
- 测试脚本应根据类别进行分组，例如接口测试与数值收敛测试各为一组。分组后的测试脚本应该保存在同一个文件夹中。
- `GROUP` 环境变量应该被用来指定在持续集成中进行并行测试的测试组。当开发人员在本地运行 `]test Package` 时，应有一个回退组 `All` 来指定所有应该被运行的测试。可以参考 [OrdinaryDiffEq.jl 的测试结构](https://github.com/SciML/OrdinaryDiffEq.jl/blob/v6.10.0/test/runtests.jl)来作为案例。
- 测试应该包括对使用该功能的下游主要软件包的测试，以确保持续支持下游软件包。任何破坏下游测试的更新都应向下游软件包发出通知，说明支持被破坏的原因（最好以修复支持的PR的形式），如果更改的功能是公共API的一部分，则应在下一版本中对软件包进行重大版本升级。
- 配置信息脚本应该使用默认配置，除非有特殊需求。
- CI脚本应该测试长期支持（LTS）版本和当前稳定版本。每夜测试仅对特定编译器细节有严重依赖的软件包才是必要的。
- 任何支持 GPU 的软件包都应该包含针对 GPU 的持续集成。
- 应该启用[文档测试](https://juliadocs.github.io/Documenter.jl/stable/man/doctests/)，除了那些由于计算上的限制而不能作为持续集成的一部分的示例。

### 空格

- 避免在括号、方括号或大括号的内部出现多余的空格。

    ```julia
    # Yes:
    spam(ham[1], [eggs])

    # No:
    spam( ham[ 1 ], [ eggs ] )
    ```

- 避免在逗号或分号之前加入多余的空格：

    ```julia
    # Yes:
    spam(ham[1], [eggs])

    # No:
    spam( ham[ 1 ], [ eggs ] )
    ```

- 避免在范围 `:` 周围留有空格。使用括号来明确任何一侧的表述。

    ```julia
    # Yes:
    ham[1:9]
    ham[9:-3:0]
    ham[1:step:end]
    ham[lower:upper-1]
    ham[lower:upper - 1]
    ham[lower:(upper + offset)]
    ham[(lower + offset):(upper + offset)]

    # No:
    ham[1: 9]
    ham[9 : -3: 1]
    ham[lower : upper - 1]
    ham[lower + offset:upper + offset]  # Avoid as it is easy to read as `ham[lower + (offset:upper) + offset]`
    ```

- 避免在赋值（或其他）操作符周围使用多个空格以将其与其他操作符对齐：

    ```julia
    # Yes:
    x = 1
    y = 2
    long_variable = 3

    # No:
    x             = 1
    y             = 2
    long_variable = 3
    ```

- 在大多数二进制运算符的两侧加上一个空格：赋值运算符（`=`），[更新运算符](https://docs.julialang.org/en/v1/manual/mathematical-operations/#Updating-operators-1)（`+=`，`-=` 等），[数值比较运算符](https://docs.julialang.org/en/v1/manual/mathematical-operations/#Numeric-Comparisons-1)（`==`，`<`，`>`，`!=` 等），[lambda 运算符](https://docs.julialang.org/en/v1/manual/functions/#man-anonymous-functions-1)（`->`）。不包括以下二元运算符： [范围运算符 ](https://docs.julialang.org/en/v1/base/math/#Base.::)（`:`）、 [有理数运算符](https://docs.julialang.org/en/v1/base/math/#Base.://) （`//`）、[指数运算符](https://docs.julialang.org/en/v1/base/math/#Base.:^-Tuple{Number,%20Number})（`^`）以及[可选参数/关键字](https://docs.julialang.org/en/v1/manual/functions/#Optional-Arguments-1)（例如 `f(x = 1; y = 2)`）。请勿将这些运算符包含在本准则中。

    ```julia
    # Yes:
    i = j + 1
    submitted += 1
    x^2 < y

    # No:
    i=j+1
    submitted +=1
    x^2<y
    ```

- 避免在一元操作数和表达式之间使用空格：

    ```julia
    # Yes:
    -1
    [1 0 -1]

    # No:
    - 1
    [1 0 - 1]  # Note: evaluates to `[1 -1]`
    ```

- 避免多余的空行。避免在单行方法定义之间留空行，而在其他独立的函数之间使用一个空行分隔，如果需要的话，可以加上注释。

    ```julia
    # Yes:
    # Note: an empty line before the first long-form `domaths` method is optional.
    domaths(x::Number) = x + 5
    domaths(x::Int) = x + 10
    function domaths(x::String)
        return "A string is a one-dimensional extended object postulated in string theory."
    end

    dophilosophy() = "Why?"

    # No:
    domath(x::Number) = x + 5

    domath(x::Int) = x + 10



    function domath(x::String)
        return "A string is a one-dimensional extended object postulated in string theory."
    end


    dophilosophy() = "Why?"
    ```

- 超过单行字符限制的函数调用应该被分成多行，以使包含左括号和右括号的行具有相同的缩进，而函数的参数则再缩进一级。在大多数情况下，每个参数和/或关键字应该分别放在不同的行上。请注意，这个规则与 Julia 的典型约定冲突，即，与缩进下一行以与包含参数的开放括号对齐这一点冲突。如果你为具有不同规约的程序包编写代码，请遵循软件包中使用的规约，而不是使用本指南。

    ```julia
    # Yes:
    f(a, b)
    constraint = conic_form!(SOCElemConstraint(temp2 + temp3, temp2 - temp3, 2 * temp1),
                             unique_conic_forms)

    # No:
    # Note: `f` call is short enough to be on a single line
    f(
        a,
        b,
    )
    constraint = conic_form!(SOCElemConstraint(temp2 + temp3,
                                               temp2 - temp3, 2 * temp1),
                             unique_conic_forms)
    ```

- 将类似的单行语句分组在一起。

    ```julia
    # Yes:
    foo = 1
    bar = 2
    baz = 3

    # No:
    foo = 1

    bar = 2

    baz = 3
    ```

- 请使用空行分隔不同的多行文本块。

    ```julia
    # Yes:
    if foo
        println("Hi")
    end

    for i in 1:10
        println(i)
    end

    # No:
    if foo
        println("Hi")
    end
    for i in 1:10
        println(i)
    end
    ```
- 定义完一个函数后，请不要在结束语句前添加空行。

    ```julia
    # Yes:
    function foo(bar::Int64, baz::Int64)
        return bar + baz
    end

    # No:
    function foo(bar::Int64, baz::Int64)

        return bar + baz
    end

    # No:
    function foo(bar::In64, baz::Int64)
        return bar + baz

    end
    ```

- 在控制流语句和返回语句之间使用换行符。

    ```julia
    # Yes:
    function foo(bar; verbose = false)
        if verbose
            println("baz")
        end

        return bar
    end

    # Ok:
    function foo(bar; verbose = false)
        if verbose
            println("baz")
        end
        return bar
    end
    ```

### 命名元组

在 `NamedTuple` 中，`=` 字符应该像关键字参数一样有空格。参数名称与其值之间应该留空格。空 `NamedTuple` 应该写入 `NamedTuple()` 而不是 `(;)`

```julia
# Yes:
xy = (x = 1, y = 2)
x = (x = 1,)  # Trailing comma required for correctness.
x = (; kwargs...)  # Semicolon required to splat correctly.

# No:
xy = (x=1, y=2)
xy = (;x=1,y=2)
```

### 数值

- 浮点数应始终写出小数点前面和/或后面的零：

```julia
# Yes:
0.1
2.0
3.0f0

# No:
.1
2.
3.f0
```

- 始终优先选择类型 `Int` 而不是 `Int32` 或 `Int64`，除非有特定原因需要选择位大小。

### 三元操作符

三元运算符（`?:`）通常只应占一行。不要连续嵌套多个三元运算符。如果连续嵌套了多个三元操作符，请考虑使用类似 `if`-`elseif`-`else` 这样的条件语句、派发或是字典。

```julia
# Yes:
foobar = foo == 2 ? # Yes:
foobar = foo == 2 ?
    bar :
    baz
foobar = foo == 2 ? bar : foo == 3 ? qux : baz
```

还有一种替代方案，你可以使用复合布尔表达式：

```julia
# Yes:
foobar = if foo == 2
    bar
else
    baz
end

foobar = if foo == 2
    bar
elseif foo == 3
    qux
else
    baz
end
```

### For 循环

循环应该始终使用 `in`，而不是 `=` 或 `∈`。这也适用于列表和生成器推导式。

```julia
# Yes
for i in 1:10
    #...
end

[foo(x) for x in xs]

# No:
for i = 1:10
    #...
end

[foo(x) for x ∈ xs]
```

### 函数类型标注

函数定义的类型标注应尽可能通用。

```julia
# Yes:
splicer(arr::AbstractArray, step::Integer) = arr[begin:step:end]

# No:
splicer(arr::Array{Int}, step::Int) = arr[begin:step:end]
```

尽可能使用尽可能多的通用类型，使函数可以允许多种输入，并使您的代码更加通用：

```julia
julia> splicer(1:10, 2)
1:2:9

julia> splicer([3.0, 5, 7, 9], 2)
2-element Array{Float64,1}:
 3.0
 7.0
```

### 结构类型标注

对类型字段的标注需要更加深思熟虑，因为除非编译器可以推断出类型，否则字段访问是不确定的（有关详细信息，请参见[类型调度设计](https://www.stochasticlifestyle.com/type-dispatch-design-post-object-oriented-programming-julia/)）。由于易于推断的代码是首选，因此抽象类型标注，即。

```julia
mutable struct MySubString <: AbstractString
    string::AbstractString
    offset::Integer
    endof::Integer
end
```

是不推荐的。相反，使用具体类型的结构体：

```julia
mutable struct MySubString <: AbstractString
    string::String
    offset::Int
    endof::Int
end
```

是推荐的。如果需要保持通用性，则首选参数化类型，如：

```julia
mutable struct MySubString{T<:Integer} <: AbstractString
    string::String
    offset::T
    endof::T
end
```

未指定类型的字段应明确指定为 `Any`，即：

```julia
struct StructA
    a::Any
end
```

### 宏

- 在有多个赋值时不要在赋值之间加上空格。

```julia
Yes:
@parameters a = b
@parameters a=b c=d

No:
@parameters a = b c = d
```

### 类型与类型标注

- 避免使用复杂的 union 类型。`Vector{Union{Int,AbstractString,Tuple,Array}}` 应该改为 `Vector{Any}`。这将减少编译检查过多分支时造成的额外压力。
- 分支拆分时，应只保留两到三种类型的 Unions. 只有三种类型的 Unions 一般能将编译时间减少到最小。
- 不要使用 `===` 来比较类型。请使用 `isa` 或 `<:` 来替代。

### 软件包版本规范

- 使用[语义化版本控制](https://semver.org/)
- 为了简洁起见，在指定软件包依赖版本时避免写入默认的插入符号。

```julia
# Yes:
DataFrames = "0.17"

# No:
DataFrames = "^0.17"
```

- 为了保证准确性，请不要使用诸如 `>=` 之类的结构来限制版本号。
- 每个依赖都应该有版本锁定。
- 所有包都应该使用 [CompatHelper](https://github.com/JuliaRegistries/CompatHelper.jl) 并始终尝试使用依赖的最新版本。
- 依赖关系的下限应该是最后的测试版本。

### 文档

- 文档应始终尽量在最高级编写。换言之，倾向于编写一个适用于所有方法的接口文档，而不是为每个方法都编写文档，倾向于为抽象类型编写接口文档，而不是为每个子类型都编写文档。所有实例应该指向更高层级的文档。
- 文档应该使用 [Document.jl](https://juliadocs.github.io/Documenter.jl/stable/)  来构建。
- 教程应该写在参考材料之前。
- 每个包都应该有一个入门教程，并且能够涵盖“90%的用例”，即包含大多数人会想要使用包的方式。
- 教程应该展示一个完整的工作流，并明确写出该工作流选择使用什么并如何操作。例如，当撰写一个关于仿真器的教程时，应该明确指出使用哪个绘图包并展示如何用其进行绘图。
- 教程中的变量名很重要。如果您使用 `u0`，那么所有其他代码都应使用相同的命名。好向潜在用户展示如何以正确的命名方式正确的使用您的代码。
- 若情况合适，如何使用“高性能高级功能”的教程应该与入门教程分开。
- 在开始描述 API 文档字符串的具体细节之前，所有文档都应该总结其内容。
- 大多数模块、类型和函数应该编写[文档字符串](http://docs.julialang.org/en/v1/manual/documentation/)。
- 文档应尽可能提供访问函数而不是相关字段的链接。文档化后的字段是公共 API 的一部分，更改其内容/名称将构成破坏性更改。
- 只有导出的函数才需要进行文档化。
- 避免编写常常被重载的方法，例如 `==`。
- 尽量在可能的情况下为一个函数而不是单独的方法编写文档，因为通常所有的方法都会有相似的文档字符串。
- 如果您正在为已经有文档字符串的函数添加一个方法，只有在您的函数行为与现有的文档字符串不一致时，才添加一个新的文档字符串。
- 文档字符串应使用 [Markdown](https://en.wikipedia.org/wiki/Markdown) 语法编写，且内容需简明扼要。
- 文档字符串的单行长度不超过92个字符。

```julia
"""
    bar(x[, y])

Compute the Bar index between `x` and `y`. If `y` is missing, compute the Bar index between
all pairs of columns of `x`.
"""
function bar(x, y) ...
```

- 当内容足太长时，建议在标题和内容之间留出一个空行。
- 请尝试在文档字符串中保持风格一致，无论您是否使用了这个额外的空格。
- 尽可能遵循下列模板：

类型模板（如果与构造函数的文档字符串重复则应跳过）：

```julia
"""
    MyArray{T, N}

My super awesome array wrapper!

# Fields
- `data::AbstractArray{T, N}`: stores the array being wrapped
- `metadata::Dict`: stores metadata about the array
"""
struct MyArray{T, N} <: AbstractArray{T, N}
    data::AbstractArray{T, N}
    metadata::Dict
end
```

函数模板（仅适用于导出函数）：

```julia
"""
    mysearch(array::MyArray{T}, val::T; verbose = true) where {T} -> Int

Searches the `array` for the `val`. For some reason we don't want to use Julia's
builtin search :)

# Arguments
- `array::MyArray{T}`: the array to search
- `val::T`: the value to search for

# Keywords
- `verbose::Bool = true`: print out progress details

# Returns
- `Int`: the index where `val` is located in the `array`

# Throws
- `NotFoundError`: I guess we could throw an error if `val` isn't found.
"""
function mysearch(array::AbstractArray{T}, val::T) where {T}
    ...
end
```

- 只要有 LaTeX 语句，就应该使用 Markdown 标准库中的 `@doc doc""" """` 语法。
- 只能为类型的公共字段编写文档。未编写文档的字段被视为非公开的内部内容。
- 如果您的方法包含大量的参数或关键字，您可能希望在第一行方法签名中将它们排除在外，换而使用 `args...` 和/或 `kwargs...`。

```julia
"""
    Manager(args...; kwargs...) -> Manager

A cluster manager which spawns workers.

# Arguments

- `min_workers::Integer`: The minimum number of workers to spawn or an exception is thrown
- `max_workers::Integer`: The requested number of workers to spawn

# Keywords

- `definition::AbstractString`: Name of the job definition to use. Defaults to the
    definition used within the current instance.
- `name::AbstractString`: ...
- `queue::AbstractString`: ...
"""
function Manager(...)
    ..
end
```

- 可以随意的在同一个文档字符串中记录一个函数的多种方法。但请只为你已定义的函数这么做。

```julia
"""
    Manager(max_workers; kwargs...)
    Manager(min_workers:max_workers; kwargs...)
    Manager(min_workers, max_workers; kwargs...)

A cluster manager which spawns workers.

# Arguments

- `min_workers::Int`: The minimum number of workers to spawn or an exception is thrown
- `max_workers::Int`: The requested number of workers to spawn

# Keywords

- `definition::AbstractString`: Name of the job definition to use. Defaults to the
    definition used within the current instance.
- `name::AbstractString`: ...
- `queue::AbstractString`: ...
"""
function Manager end

```

- 如果文档中的项目符号超过92个字符，则应将该行换行并进行缩进。请避免将文字对齐到`：`。

```julia
"""
...

# Keywords
- `definition::AbstractString`: Name of the job definition to use. Defaults to the
    definition used within the current instance.
"""
```

### 报错处理

- `error("string")` 应予避免。尽可能定义和抛出异常类型。请参阅[有关异常的文档，以获取更多详细信息](https://docs.julialang.org/en/v1/manual/control-flow/#Exception-Handling)。
- 尝试避免使用 `try/catch`。请尽量少用它。应该在运行代码之前就处理好潜在问题。

### 数组

- 尽可能避免使用 splating 语法（`...`）。优先使用迭代器，如 `collect`、 `vcat`、 `hcat` 等。

### 行尾

总是遵循 Unix 风格，在代码最后一行添加 `\n`。

### VS-Code 配置

如果您是VS Code的用户，我们建议您在Julia语法特定设置中使用以下选项。要使用这些设置，请使用快捷键 <kbd>CMD</kbd>+<kbd>,</kbd> （Mac OS）或 <kbd>CTRL</kbd>+<kbd>,</kbd> （其它系统）打开你的 VS Code 设置，然后将以下代码添加至 `settings.json` 中：

```json
{
    "[julia]": {
        "editor.detectIndentation": false,
        "editor.insertSpaces": true,
        "editor.tabSize": 4,
        "files.insertFinalNewline": true,
        "files.trimFinalNewlines": true,
        "files.trimTrailingWhitespace": true,
        "editor.rulers": [92],
        "files.eol": "\n"
    },
}
```
此外，您可能会发现 [Julia VS-Code 拓展](https://github.com/julia-vscode/julia-vscode) 很有用。

### JuliaFormatter

**注意：** `sciml` **风格只在 ** `JuliaFormatter v1.0` **或更高版本中可用**

添加以下内容到 `.JuliaFormatter.toml`
```toml
style = "sciml"
```
并在仓库根目录下运行
```julia
using JuliaFormatter, SomePackage
format(joinpath(dirname(pathof(SomePackage)), ".."))
```
得以自动格式化软件包代码

添加 [FormatCheck.yml](https://github.com/SciML/ModelingToolkit.jl/blob/master/.github/workflows/FormatCheck.yml) 来启用 CI 格式化检测。当代码不符合风格规范时，CI 会失败。因此，在提交之前应该先执行 `格式化`。

# 参考

本文许多准则参考自 [Julia style guide](https://docs.julialang.org/en/v1/manual/style-guide/)，[YASGuide](https://github.com/jrevels/YASGuide)，以及 [Blue style guide](https://github.com/invenia/BlueStyle#module-imports).
