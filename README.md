下記は https://github.com/Kayla-Morrell/CreateAPackage のvignetteを日本語に翻訳したものです。

# Create A Package (パッケージを作る)

A package is a collection of functions that work together in a cohesive manner 
to achieve a goal. It (should) include detailed documentation through man pages 
and vignettes and (should) be tested for accuracy and efficiency. [Writing R extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html) 
is a very detailed manual provided by CRAN that explains writing packages and 
what the structure of a package should look like. This vignette is going to walk 
you through making your first package in RStudio as well as how to submitt a 
package to _Bioconductor_.

パッケージは、目的を達成するためにまとまった方法で連携する関数のコレクションです。
manページとvignetteによる詳細なドキュメントが含まれていて (含まれているべきで)、
正確さと効率のためにテストされています (テストされるべきです)。
[Writing R extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html)
はCRANが提供する非常に詳細なマニュアルであり、パッケージの作成とパッケージの構造が
どのように見えるかを説明しています。
このvignetteでは、RStudioで最初のパッケージを作成する方法と、
_Bioconductor_ にパッケージを投稿する方法について説明します。

## Using `devtools` to create a package (パッケージを作るために `devtools`を使うことについて)

The `devtools` package provides a lot of options and utility for helping to 
construct a new package. You can get a list of all available `devtools` functions 
with `ls("package:devtools")`.

`devtools` パッケージは、新しいパッケージの構築を支援するための多くのオプションとユーティリティ
を提供します。
`ls("package:devtools")` を使用すると、利用可能なすべての`devtools`の関数のリストを取得できます。

Some useful references for using `devtools` to build packages are [Rstudio Devtools Cheetsheet](https://www.rstudio.com/wp-content/uploads/2015/03/devtools-cheatsheet.pdf) and [Jennifer Bryan class](http://stat545.com/packages06_foofactors-package.html).

`devtools` を使用してパッケージをビルドするための便利なリファレンスには、
[Rstudio Devtools Cheetsheet](https://www.rstudio.com/wp-content/uploads/2015/03/devtools-cheatsheet.pdf) や
[Jennifer Bryan class](http://stat545.com/packages06_foofactors-package.html) があります。

### Package shell
```{r, eval = FALSE}
library(devtools)
create("myFirstPackage")
```

`create()` will _create_ all the necessary files and sub-directories that are 
required by R to be a valid package: DESCRIPTION, NAMESPACE, and R directory. 
The DESCRIPTION file contains the basic information about the package and you 
will have to edit so that the information is pertinent to your package. The 
NAMESPACE file describes which functions will be imports and exports of the 
namespace. The R directory will contain the R code files for your package.

`create()` は、有効なパッケージであるためにRによって要求される
すべての必要なファイルとサブディレクトリ(DESCRIPTION、NAMESPACE、およびRディレクトリ)
を _作成_ します。
DESCRIPTIONファイルには、パッケージについての基本情報が含まれており、あなたはその情報が
あなたのパッケージに関するものになるように編集する必要があります。
NAMESPACEファイルは、どの関数をインポートするかや名前空間のエクスポートを表します。
Rディレクトリには、あなたのパッケージのRコードファイルが含まれます。

After running `create()`, the package has a vaild package structure which means 
it can be installed and loaded:

`create()` を実行すると、そのパッケージは有効なパッケージ構造になり、
インストールおよびロードできるようになります。
 
* `library("myFirstPackage")`,
* `library(help="myFirstPackage")`.

### Version control (バージョン管理)

It is an excellent idea to version control whenever creating a package and 
especially when collaborating on a project, where multiple users are allowed to 
make changes. Version control allows for a constant recored of changes that can 
be advanced or reverted if necessary.

パッケージを作るときはいつでもバージョン管理を行うことをお勧めします。
複数のユーザーが変更を加えることが許可されているプロジェクトで共同作業を行う場合は特にお勧めします。
バージョン管理により、必要に応じて拡張または元に戻すことができる変更の一定した記録が可能になります。

Only a project can be version controlled and to make a directory a project in 
RStudio go to: `File -> New Project`. In this case we started creating the 
directory so we will follow the prompts for the option `Use Existing Directory`. 
Now that it is a project we can go to `Tools -> Version Control -> Project Setup` 
and change the `Version Control System` to `Git`, then follow the prompts. 
Notice in the RStudio pane for environments/history/build there is a new tab 
named 'Git'. The package can now start using `git` version control through 
making commits. To make a commit, you can go to the Git tab, select the check 
box next to any files that have been modified, added, or deleted that you would 
like to track, and select `commit`. Enter a new commit message in the window 
that pops up and select `commit`.

バージョン管理できるのは「プロジェクト」のみで、RStudioで あるディレクトリを「プロジェクト」
にするには、 `File -> New Project` に移動します。
この場合、すでにディレクトリの作成を開始した(訳注:後な)ので、`Use Existing Directory` オプションのプロンプトに従います。
これでプロジェクトができたので、`Tools -> Version Control -> Project Setup` に移動して、
`Version Control System` を `Git` に変更し、プロンプトに従います。
RStudio の environments/history/build のペインに、'Git' という名前の新しいタブがあることに注意してください。
パッケージはコミットを行い、 `git` のバージョン管理を使い始めることができるようになりました。
コミットを行うには、Git タブに移動し、変更、追加、または削除されたファイルの横にあるチェックボックスを選択し、`commit` を選択します。
そしてポップアップウィンドウに新しいコミットメッセージを入力し、 `commit` を選択しましょう。

It is important to tell Git just who we are. In RStudio, select `Tools -> Shell` 
and type the following making sure to substitue *your* user.email and user.name.
If you have github we recommend using your email and user.name associated with 
github here. 

私たちが誰であるかをGitに伝えることは重要です。
RStudioで、`Tools -> Shell` を選択し、次のように入力して、必ず *あなたの* 
user.email と user.name に置き換えてください。
github をお使いの場合は、githubに関連付けられているメールアドレスと user.name 
をここで使用することをお勧めします。

```
git config --global user.email "<someemail@gmail.com>"
git config --global user.name "<githubUserName>"
```

We could stop here but we also would like to put the package on GitHub. These 
next steps assume you have a github account. First, in RStudio go to `Tools -> 
Global Options` and select Git/SVN. Ensure the paths are correct. If you have 
not linked an RStudio project to GitHub, select `Create RSA key`. Close the 
window. Click on `View public key` and copy the displayed public key. Now in a 
web browser, open your GitHub accout. Go to `Settings` and `SSH and GPG keys`. 
Click on the option for `New SSH key` and paste the public key that was copied. 
Also on GitHub, create a new repository with the same name as the one you 
created with `create()` in RStudio. Back in RStudio, select `Tools -> Shell` and 
type the following making usre to substitue your GitHub user.name and the new 
package name.

ここで止めることもできますが、パッケージを GitHub に配置することもできます。
次の手順では、あなたがgithubアカウントを持っていることを前提としています。
まず、RStudioで `Tools -> Global Options` に移動し、Git/SVN を選択します。
パスが正しいことを確認してください。
もしRStudioのプロジェクトをGitHubにリンクしていない場合は、 `Create RSA key` を選択します。
そしてウインドウを閉じてください。
次に `View public key` をクリックして、表示された公開鍵をコピーします。
次に、WebブラウザーでGitHubアカウント(訳注:のwebページ)を開きます。
`Settings` と `SSH and GPG keys` に移動します。
`New SSH key` のオプションをクリックして、コピーした公開鍵を貼り付けます。
また、GitHubで、RStudioで `create()` で作ったものと同じ名前で新しいリポジトリを作成します。
RStudioに戻り、 `Tools -> Shell` を選択し、次のように入力して、GitHub の user.name
と新しいパッケージ名を確実に置き換えてください。

```
git remote add origin https://github.com/<github user.name>/<package repo name>.git
git remote set-url origin git@github.com:<github user.name>/<package repo name>.git
git pull origin master
git push -u origin master
```

For instance, this is what the commands would look like for me:

たとえば、私用のコマンドはこのようになります。

```
git remote add origin https://github.com/Kayla-Morrell/myFirstPackage.git
git remote set-url origin git@github.com:Kayla-Morrell/myFirstPackage.git
git pull origin master
git push -u origin master
```

The `git remote add` command will create a new connection to the remote 
repository url and assign it the shortname 'origin' for easy referencing moving 
forward. Then the `git remote set-url` command is going to switch the 
url of the remote from 'https', which is public read only access, to 'SSH' so 
that you as a developer can have read and write access to the repository. `git 
pull` is going to fetch and download content from the remote repository and 
update the local repository to match. `git push` does the exact opposite, it 
will upload the local repo changes to the remote repo.

`git remote add` コマンドは、リモートリポジトリのURLへの新しい接続を作成し、
簡単に参照できるように短縮名 'origin' を割り当てます。
次に、 `git remote set-url` コマンドは、リモートのURLを  'https'
(パブリック読み取り専用アクセス) から 'SSH' に切り替えて、
あなたが開発者としてリポジトリへの読み書きのアクセスを行えるようにします。
`git pull` は、リモートリポジトリからコンテンツをフェッチ・ダウンロード
し、(訳注:リモートリポジトリに)一致するようにローカルリポジトリを更新します。
`git push` はまったく逆のことを行い、ローカルリポジトリの変更を
リモートリポジトリにアップロードします。

Now if you look in the RStudio tab for Git, the push and pull options are 
available. You can now push and pull from/to the local and GitHub repository 
version of your package.

Git用の RStudio のタブを見ると、プッシュとプルのオプションがあります。
これで、パッケージのローカルと GitHub のリポジトリバージョン から/へ 
プッシュおよびプルできます。

## DESCRIPTION file

`devtools` provides built in functions for building, checking and installing a 
package. The package we created earlier using `create()` has a valid package 
structure but if we did `check()` we will find the DESCRIPTION file needs to be 
updated. The information in the DESCRIPTION file needs to be reflective of your 
package. These are the fields that should be changed:

`devtools` は、パッケージをビルド、チェック、インストールするための組み込み関数を提供します。
以前に `create()` を使って作ったパッケージは有効なパッケージ構造を持ちますが、
`check()` を実行すると DESCRIPTION ファイルを更新する必要があることがわかります。
DESCRIPTION ファイルの情報は、あなたのパッケージ(訳注:の内容)を反映している必要があります。
変更する必要があるフィールドは次のとおりです:

* Title: Should be brief, but descriptive. 簡潔にする必要がありますが、説明を含むべきです。
* Version: Should be 0.99.0 for pre-release packages. プレリリースパッケージの場合は 0.99.0 にします。
* Authors@R: Only one person should be listed as maintainer (`cre`). We do 
accept Author/Maintainer for this field, either can be used but not both.
メンテナ (`cre`) としてリストされるのは1人だけです。このフィールド
には Author/Maintainer を受け入れます。どちらかを使えますが、両方は使えません。
* Description: Should be relatively short but a detailed overview of what the 
package functionality entails. 
比較的短くする必要がありますが、パッケージ機能に必要なものの詳細な概要です。
* License: Preferably a standard license, Bioconductor core packages typically 
use Artistic-2.0. If using a non-standard license, must include a LICENSE file 
in your package and use `file LICENSE` in this field.
できれば標準のライセンスにしてください。Bioconductor のコアパッケージは、
通常 Artistic-2.0 を使います。非標準のライセンスを使用する場合は、パッケージに
LICENSE ファイルを含め、このフィールドで `file LICENSE` を使う必要があります。
* LazyData: Should be set to false, we find that this field being true can slow 
down the loading of a package especially if it contains large data.
false に設定する必要があります。このフィールドをtrueにすることは、
特に大きなデータが含まれている場合にパッケージの読み込みを遅くする可能性があります。

Throughout the development of your package you may have to update the 
DESCRIPTION file for appropriate Depends, Imports, and Suggests fields as you 
incorporate more functionality from other packages. As the package is developing 
we also require having a `biocViews:` field in the DESCRIPTION file. This field 
will contain at least two biocViews categories that reflect the nature of the 
package.

パッケージの開発中は、他のパッケージからより多くの機能を組み込むように、
適切な Depends, Imports, そして Suggests フィールドのために DESCRIPTION ファイル
を更新しなければならないかもしれません。
パッケージの開発中は、DESCRIPTIONファイルに `biocViews:` フィールドも必要です。
このフィールドはパッケージの性質を反映する少なくとも2つの biocViews カテゴリ
を含みます。

## R functions

Now we want to starting writing R functions. In RStudio you can open an empty 
file by doing `File -> New File` and selecting `R Script`. Save the file in the 
R directory. Write your functions and document. You can either document 
functions manually or if you use roxygen you can use the devetools function 
`document()`. See the [Writing R extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html) 
for manual creation of Rd files, which belong in the man directory, but roxygen 
is growing increasingly popular. Some helpful links for roxygen tags can be 
found at [RStudio Devtools Cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/devtools-cheatsheet.pdf) and 
[Roxygen Help](https://cran.rstudio.com/web/packages/roxygen2/vignettes/rd.html).

次に、R の関数を書くことを始めます。
RStudioでは、 `File -> New File` を実行してRスクリプトを選択することにより、空のファイルを開くことができます。
そのファイルをRディレクトリに保存しましょう。
次にあなたの関数とドキュメントを書きましょう。
関数のドキュメントを書くのは手作業でもできますし、roxygenを使うならdevtoolsの `document()` 関数を使うこともできます。
manディレクトリにあるRdファイルの手作業の作成には、[Writing R extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html)
を参照してください。
ただしroxygenの人気が次第に高まりつつあります。
roxygen のタグに役立つリンクは、
[RStudio Devtools Cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/devtools-cheatsheet.pdf)
と 
[Roxygen Help](https://cran.rstudio.com/web/packages/roxygen2/vignettes/rd.html)
にあります。

Some useful `devtools` commands while creating functions are:

関数の作成中に役立ついくつかの `devtools` のコマンドは次のとおりです:

* `load_all()` which loads all package functions in environment to test,
テストする環境ですべてのパッケージ関数をロードします,
* `check()` which checks the package (R CMD check),
パッケージをチェックします (R CMD check を実行します),
* `document()` which generates or updates any documentation files.
ドキュメントファイルを生成または更新します。

Using the RStudio options `Build -> Build and Reload` and `Build -> Clean 
and Rebuild` will also help with function creation.

RStudio のオプションの `Build -> Build and Reload` と
`Build -> Clean and Rebuild` を使うことは、関数の作成にも役立ちます。

It is also recommended to have a man page for you package. `devtools` provides 
a framework for this. To create the file that needs to be modified use the 
function `use_package_doc()`.

パッケージ用のmanページを用意することもお勧めします。
`devtools` はこのためのフレームワークを提供します。
修正が必要なファイルを作成するには、関数 `use_package_doc()` を使います。

If you import any functions in your code, don't forget to update the DESCRIPTION 
file for Depends, Imports, or Suggests. If the function provides essential 
functionality for users of your package, it belongs in Depends. It is unusual 
for more than three packages to be listed as Depends. For packages that provide 
functions, methods, or classes that are used inside your package namespace, they 
belong in Imports. Most packages will be listed here. For packages that are used 
in vignettes, examples, or conditional code, they should be listed as Suggests. 
This includes examples that may use annotation and/or experiment packages.

コードに関数をインポートする場合は、Depends、Imports、またはSuggestsのために
DESCRIPTION ファイルを更新することを忘れないでください。
関数がパッケージのユーザーに不可欠な機能を提供する場合、それはDependsに属します。
Dependsとして3つ以上のパッケージがリストされるのは珍しいことです。
あなたのパッケージの名前空間内で使用される関数、メソッド、またはクラスを提供する
パッケージの場合、それらは Imports に属します。
ほとんどのパッケージがここにリストされます。
ビネット、例、または条件付きコードで使われるパッケージの場合、
それらは Suggests としてリストされている必要があります。
これには、annotation や experiment パッケージを使う例が含まれます。

## Testing

It is highly recommened to add unit tests to your package. Unit tests unsure 
that the package is working as expected. The two main ways to test are using 
`RUnit` or `testthat`. `testthat` functionality is included in `devtools` by 
using `use_testthat()`. This function will set up the needed directory structure 
and add the package suggestion to the DESCRIPTION file. Here are some examples 
of the structure of tests for `testthat`:

* `expect_identical()`,
* `expect_true()`,
* `expect_error()`.

There are other options as well that are discussed in [testthat Wickham](https://journal.r-project.org/archive/2011-1/RJournal_2011-1_Wickham.pdf) and [testthat](http://r-pkgs.had.co.nz/tests.html).

## Vignettes

Vignettes are another major documentation piece to a package. More and more 
repository systems (CRAN, Bioconductor, ROpenSci) are making vignettes a 
standard requirement. Vignettes are contain a more indepth description and 
examples of the package usage. `devtools` also provides the function 
`use_vignette()` to set up the directory structure and the initial file for 
a vignette. For Bioconductor submissions we recommend changing the `output:` 
section for the vignette header to the following, which would require adding 
`BiocStyle` to the Suggests field in the DESCRIPTION file, 

```
output:
  BiocStyle::html_document:
    toc: true
    toc_depth: 2
```

Or if `BiocStyle` is already installed on your system, you can also use RStudio 
to set up the vignette by doing `New File -> Rmarkdown -> From Template -> 
Bioconductor HTML/PDF Vignette`.

A helpful rmarkdown link, which is commonly used for vignette creation, can be 
found here: [rmarkdown cheatsheet](http://www.rstudio.com/wp-content/uploads/2016/03/rmarkdown-cheatsheet-2.0.pdf).

### Bioconductor Standards

Now that we have gone over the basics of how to create a package, we will review 
what we look for (generally) in Bioconductor packages. Being mindful of these 
guidelines while developing your package will help the whole submission process. 

1. Proper coding and efficient coding:

* [Efficient and Robust Code](http://bioconductor.org/developers/how-to/efficient-code/)
* [Query Web Resource](http://bioconductor.org/developers/how-to/web-query/)

2. Bioconductor interconnectivity and S4 classes

* S4 over S3 classes
* Reuse existing infrastructure first!
    + DNA/RNA - `Biostrings` `DNAstringset`
    + Gene sets - `GSEABase` `GeneSet`
    + Genomic intervals - `GenomicRanges` `Granges`
    + Rectangular feature x sample data - (RNAseq count matrix, microarray) - 
    `SummarizedExperiment`/`MuliAssayExperiment`
    + Single cell - `SingleCellExperiment`
    + Mass spec - `MSnbase`
    + import/loading data
        - GTF, GFF, BED, BigWig, ... `rtracklayer::import()`
        - FASTA `Biostrings::readDNAStringSet()`
        - SAM/BAM `Rsamtools::scanBam()`, `GenomicAlignments::readGAlignment*()`
        - VCF `VariantAnnotation::readVcf()`
        - FASTQ `ShortRead::readFastq()`
* [BiocViews](http://bioconductor.org/packages/release/BiocViews.html#___Software)

3. Tests

* [Unit test guidelines](http://bioconductor.org/developers/how-to/unitTesting-guidelines/)

4. Complete and detailed vignette(s) and man pages, with executable examples

5. Check time < 5 minutes

6. Package size < 5Mb

7. All package guidelines can be found [here](http://bioconductor.org/developers/package-guidelines/)

**IMPORTANT**: A clean build, check, and BiocCheck is not a guaranteed acceptance. 
The package will still go through a formal review process.

### Submitting to Bioconductor

Be sure to read the [Contributions Page](https://github.com/Bioconductor/Contributions) and 
when you are ready to submit open a [New Issue](https://github.com/Bioconductor/Contributions/issues). 
The Title: should be the name of your package. Once the package is approved for 
building, don't forget to set up the [remotes](https://bioconductor.org/developers/how-to/git/new-package-workflow/). 
Some details about the review process can be found [here](http://bioconductor.org/developers/package-submission/#whattoexpect).
