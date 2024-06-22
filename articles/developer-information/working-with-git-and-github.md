<!-- Filename: Working_with_git_and_github / Display title: Trabalhando com git e github -->

# Introdução

Este documento irá fornecer informações sobre como contribuir para o CMD
Joomla! com a ajuda do Git e GitHub. Se você gosta de fazer uma
alteração simples (um arquivo apenas), é mais fácil usar esta
documentação: [Usando a interface do Github para Fazer Requisições
Pull](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests "Special:MyLanguage/Using the Github UI to Make Pull Requests")
Se você gosta de adicionar mudanças mais complexas ou está apenas
interessado nisso, continue lendo!

## O que são Git e GitHub?

O Git é um sistema de controle de versão distribuído. É um sistema que
registra alterações em arquivos e mantém essas alterações em um arquivo
de histórico. Você pode sempre olhar para trás para uma versão anterior
do seu código e restaurar as alterações, se quiser. Devido ao histórico
do arquivo, o Git é muito útil quando você trabalha com muitas pessoas
juntas no mesmo projeto.

Aqui está como o GitHub pode ser usado.
<a href="https://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a> é um site que ajuda a
gerenciar Projetos Git de forma visual. Como proprietário de um projeto,
você pode alterar o código e comparar diferentes versões. Como usuário
do projeto, você pode contribuir fazendo uma Requisição Pull. Uma
Requisição Pull é uma solicitação para o proprietário puxar algum código
para o projeto. Você está oferecendo um pedaço de código (talvez uma
solução para um bug) e perguntando se o proprietário do projeto gostaria
de usá-lo. Se o proprietário gostar, ele pode fundir (adicionar) ao seu
projeto.

Joomla! usa GitHub e Git para manter seu código. Todos podem contribuir
com o software Joomla!. A URL para o projeto Joomla! CMS no GitHub é:
<a href="https://github.com/joomla/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms</a>

# Começando!

## Cadastrar-se no GitHub e instalar o Git

Primeiro, você vai precisar se registrar no
<a href="http://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a>. Isto é grátis e fácil de
se fazer. Basta seguir os passos fornecidos.

Agora que estamos registrados, precisamos instalar o Git. A versão mais
recente do Git pode ser encontrada em
<a href="http://git-scm.com" class="external free" target="_blank"
rel="nofollow noreferrer noopener">http://git-scm.com</a>. Faça o
download e abra o instalador. O Git é um programa CLI (Command Line
Interface ou Interface de Linha Comando em Português). No início isso
pode ser confuso e um pouco assustador, mas este documento irá guiá-lo
através deste processo.

Se você não é um usuário avançado, apenas execute o instalador e
precione os botões "próximo" até que o programa esteja instalado. O Git
não danificará o seu sistema. Mais tarde você pode removê-lo como
qualquer outro programa, se quiser.

Once we installed Git, we open the program called "Git Bash". A command
line will be opened. We're going to tell Git our name and email address.
Git will use this information when we contribute to a project. With the
following commands we give Git that information:

    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com

In the above commands, and all other commands given in this
documentation, each line is a new command. So you type the fist line,
press enter and then type the second line and press enter.

We are now ready to use Git and go farther with setting up our test
installation.

# Setting up a test installation

For our test installation we need a Web server program so that we can
install and run Joomla! on our computer. There are a lot of programs
that can do that such as
<a href="https://www.mamp.info/" class="external text" target="_blank"
rel="nofollow noreferrer noopener">MAMP</a> and
<a href="https://www.apachefriends.org/" class="external text"
target="_blank" rel="nofollow noreferrer noopener">XAMPP</a>. Download
an install one of them.

After the installation of such a program (I use MAMP in this
documentation), we going to install the latest version of Joomla!. In
our case, the latest version of Joomla! is not the last stable release.
The last version of Joomla! is the staging branch on GitHub. First, let
me explain a little bit more about GitHub.

## Fork and Clone Joomla!

On GitHub you can find projects, so called Repositories. Inside a
project you might find several versions. Such a version is called a
Branch. Joomla! has the following branches:

- **Staging:** This branch contains the latest bug fixes and new
  features of Joomla!
- **Master:** This branch is the current stable version of Joomla!
- **3.5-dev** This branch contains the files for Joomla! 3.5, which is
  not stable at this writing.

On our test location we going to use the **Staging** branch but if we
should use this branch directly we have problem. We cannot modify this
branch because we are not the owner of it. We are going to make a copy
of it. On GitHub this is called a Fork. We are the owner of that copy so
we can modify it. After modifying we compare our fork with the original
project. Then we can make a Pull Request for the changes we've made.
More about that later. You can Fork a branch by pressing the Fork button
on the
<a href="https://github.com/joomla/joomla-cms" class="external text"
target="_blank" rel="nofollow noreferrer noopener">Joomla! CMS Github
Repository</a>. This button is located at the right top of the page.

<img src="https://docs.joomla.org/images/6/6c/Github-fork-button.png"
decoding="async" data-file-width="1002" data-file-height="222"
width="1002" height="222" alt="Github-fork-button.png" />

After forking, we are going to install Joomla! on our local Web server.
Go to the folder were you can run files on you Web server. Most of the
program use a folder called `htdocs`. Once we are inside that folder,
press the right mouse button an click on: "Git Bash Here". The command
line will open for this location.

Type the following command to download the files from your Fork of the
Joomla! CMS to your computer. Please replace *username* with the
username you are using on GitHub.

    git clone https://github.com/username/joomla-cms.git

For all commands given in this documentation, you have to open Git via
the above described steps. Please remember that for other commands in
this documentation.

Once Git is ready, open your browser and go to the installation on your
localhost. Usually the URL is something like:
<a href="http://localhost/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener"><code>http://localhost/joomla-cms</code></a>.
You will now see the default Joomla! installation process.

## Install Joomla!

The installation of Joomla! for our local test installation is almost
the same as a regular installation. There are a two little differences.

For the database settings, the password and username are default. Most
often the username is `root` and the password is also `root` or there is
no password. If you still can't connect to the database, look in the
manual for your local Web server for the username and password.

The last difference is the final step of the installation. Normally we
must delete the installation folder to go further to the back-end or
front-end of Joomla!. For a test installation we can skip this part and
go directly to the back-end or front-end. Don't remove the installation
folder. It can be very useful when we have to install Joomla again.

# Make Your Changes

Now it is time to made our file changes to Joomla!. All changes we make
will be registered and monitored by Git. At any moment you can type the
command `git status` to see which files are modified or untracked.
Untracked means that the file at that location is new for Git.

If you made a mistake or you would like to restore a file, use this
command:

    git checkout path/to/file

If you like to remove all changes you made, use the following commands:

    git checkout .
    git clean -f -d

The first command resets all files. The second command removes untracked
files and folders.

## Publish Our Changes on GitHub

# Push the Change to the Fork

After making our changes, we have to upload our changes to our
repository on GitHub. After that, we can make a pull request with our
changes.

Uploading changes is called `push` in Git terms. Before we can do that,
we have to do something very important. We must create a new branch for
our changes. (A branch is a version of our project, remember?) If we
don't do that and made our change directly to the staging branch, the
first time there won't be a problem. But if we made changes for the
second time, and the change we made the first time are not merged yet,
all these changes will registered too as new changes.

So the first command we are going to run will create a new branch. It
will prevent the above described problem. Replace name-new-branch with
the name of the new branch. This name must be short, and can only
contain lower case letters and numbers. Do **NOT** use spaces. Instead
of spaces, use - (minus).

    git checkout -b name-new-branch

The next command tells git that all made changes are good, and are ready
to commit.

    git add --all

The following command adds our change to the branch. Please replace the
message with a short description of your changes. This description will
be the title of the pull request we are going to make.

    git commit -m "description"

The final command will push (upload) the changes to our fork. Please
replace name-new-branch with the name of the branch you made a few steps
above.

    git push origin name-new-branch

## Compare Forks and Make a Pull Request

After pushing our change to GitHub, go to your fork of the Joomla! CMS.

# Informação Extra

Because the staging version of Joomla! can change any moment, it is very
useful to have the possibility for keeping our fork up-to-date. We can
do that by adding a remote to our forked project:

    git remote add upstream https://github.com/joomla/joomla-cms.git

We now added a remote called "upstream". With the following command, Git
will search for new contribution (commits) in the staging branch we
don't have in our fork. If it found some, it will add them to our fork:

    git pull upstream staging

The changes are now only made on our local fork. To upload them to
GitHub use the following command:

    git push
