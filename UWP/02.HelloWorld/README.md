# Hello world

UWP�ł�Prism.Wndows��Hello world���쐬���܂��BHello world�̍쐬��ʂ��Ċ�{�I��Prism.Windows���g�����A�v���P�[�V�����̍쐬���@��������܂��B
Prism.Windows�ɂ́A�f�̏�Ԃ�Prism.Windows��DI�R���e�i�ł���Unity(��AutoFac)��O��Ƃ����o�[�W����������܂��B
�����ł́AUnity��O��Ƃ���Prism.Unity���g���ĉ�����܂��B

## Prism.Unity���g�����R

�f��Prism.Windows�ł͂Ȃ���DI�R���e�i���g�����R�͈ȉ��̒ʂ�ł��B

- �N���X�̃C���X�^���X�Ǘ����蓮�ł������Ȃ�����
- �N���X�Ԃ̈ˑ��֌W�̒������蓮�ł������Ȃ�����

����ɐs���܂��B�Ⴆ�΁AMainPageViewModel�ŁAPrism�̉�ʑJ�ڋ@�\�ł���INavigationService���g���Ƃ�DI�R���e�i���g��Ȃ��ꍇ�͈ȉ���2�X�e�b�v�𓥂݂܂��B

- �R���X�g���N�^��INavigationService���󂯎��悤��MainPageViewModel�����
- App�N���X��MainPageViewModel�̐������W�b�N��ǉ�����INavigationService��n���悤�ɂ���

DI�R���e�i���g���ƈȉ��̎菇�ɂȂ�܂��B

- �R���X�g���N�^��INavigationService���󂯎��悤��MainPageViewModel�����

�ȏ�ł��B

������Ƃ������ł����A�ˑ�����T�[�r�X�������Ă�����A�y�[�W���������Ă���ƕ��S�����`�I�ɑ����Ă��܂��܂��B���̂��߁A�����ł�Prism.Unity��O���Hello world���쐬���܂��B

## �v���W�F�N�g�̍쐬�ƎQ�Ƃ̐ݒ�

HelloWorldApp�Ƃ������O��UWP�̃v���W�F�N�g�����܂��B�����āANuGet����Prism.Unity��ǉ����܂��B

## App�N���X�̏�������

����App�N���X�����������܂��BPrismUnityApplication�N���X���p������悤��App�N���X��ύX���܂��B
�����āAOnLauncheApplicationAsync���I�[�o�[���C�h���ĉ�ʑJ�ڂ̂��߂̃T�[�r�X�ł���NavigationService���g����Main�y�[�W�֑J�ڂ���悤�ɏ������L�q���܂��B

```cs
using Prism.Unity.Windows;
using System.Threading.Tasks;
using Windows.ApplicationModel.Activation;

namespace HelloWorldApp
{
    /// <summary>
    /// ����� Application �N���X��⊮����A�v���P�[�V�����ŗL�̓����񋟂��܂��B
    /// </summary>
    sealed partial class App : PrismUnityApplication
    {
        /// <summary>
        /// �P��A�v���P�[�V���� �I�u�W�F�N�g�����������܂��B����́A���s�����쐬�����R�[�h��
        ///�ŏ��̍s�ł��邽�߁Amain() �܂��� WinMain() �Ƙ_���I�ɓ����ł��B
        /// </summary>
        public App()
        {
            this.InitializeComponent();
        }

        protected override Task OnLaunchApplicationAsync(LaunchActivatedEventArgs args)
        {
            this.NavigationService.Navigate("Main", null);
            return Task.CompletedTask;
        }

    }
}
```

��{�N���X�̕ύX�ɔ���App.xaml�̃^�O�����������܂��B

```xml
<prism:PrismUnityApplication x:Class="HelloWorldApp.App"
                             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                             xmlns:local="using:HelloWorldApp"
                             xmlns:prism="using:Prism.Unity.Windows"
                             RequestedTheme="Light">

</prism:PrismUnityApplication>
```

## Main�y�[�W�̍쐬

Prism�ł́A�f�t�H���g��Views/xxxxPage�Ƃ������O(xxxx��NavigationService�ɓn����������)�̉�ʂɑJ�ڂ��܂��B����̏ꍇ"Main"��n���Ă���̂�MainPage�ɂȂ�܂��B
�f�t�H���g�ō쐬����Ă���MainPage.xaml���폜���܂��B
�����āAViews���O��Ԃ��쐬����MainPage.xaml���쐬���܂��B

## ViewModel�̍쐬

MainPage�ɕR�Â�ViewModel���쐬���܂��B
Prism�ł̓f�t�H���g��Views/xxxxPage�ɑ΂���ViewModels/xxxxPageViewModel�Ƃ������O��ViewModel�����蓖�Ă�@�\������܂��B
���̖����K��ɉ������߂ɁAViewModels���O��Ԃ��쐬���āAMainPageViewModel�Ƃ����N���X���쐬���܂��B

�����Hello world�Ȃ̂ŊȒP�ȓ��͂��󂯎���ă��b�Z�[�W�����H���ďo�͂���ViewModel�ɂ��܂����B

```cs
using Prism.Commands;
using Prism.Windows.Mvvm;

namespace HelloWorldApp.ViewModels
{
    class MainPageViewModel : ViewModelBase
    {
        private string input;

        public string Input
        {
            get { return this.input; }
            set { this.SetProperty(ref this.input, value); }
        }

        private string output;

        public string Output
        {
            get { return this.output; }
            set { this.SetProperty(ref this.output, value); }
        }

        public DelegateCommand CreateOutputCommand { get; }

        public MainPageViewModel()
        {
            this.CreateOutputCommand = new DelegateCommand(() =>
                {
                    this.Output = $"{Input}�����͂���܂���";
                },
                () => !string.IsNullOrWhiteSpace(this.Input))
                .ObservesProperty(() => this.Input);
        }
    }
}
```

�����I�Ȃ̂́ADelegateCOmmand��Input�ɉ������͂���ĂȂ��Ǝ��s�ł��Ȃ��_�ł��B�܂��APrism 6����ǉ����ꂽObserveProperty���\�b�h���g����Input�v���p�e�B�ɕύX������x�Ɏ��s�ۂ𔻒f����悤�ɂ��Ă��܂��B

## View��ViewModel�̕R�Â�

View��ViewModel��R�Â���ɂ�ViewModelLocator�N���X���g���܂��B
ViewModelLocator��AutoWiredViewModel�Y�t�v���p�e�B��True�ɐݒ肷�邱�Ƃŋ@�\���L��������܂��B

```cs
<Page x:Class="HelloWorldApp.Views.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:HelloWorldApp.Views"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:mvvm="using:Prism.Windows.Mvvm"
      mvvm:ViewModelLocator.AutoWireViewModel="True"
      mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    </Grid>
</Page>
```

�����DataContext�Ɏ����I��MainPageViewModel���ݒ肳��܂��B
�R���p�C�����f�[�^�o�C���f�B���O���g�����߂�MainPage�̃R�[�h�r�n�C���h�Ƀv���p�e�B���`���܂��B

```cs
using HelloWorldApp.ViewModels;
using Windows.UI.Xaml.Controls;

namespace HelloWorldApp.Views
{
    /// <summary>
    /// ���ꎩ�̂Ŏg�p�ł���󔒃y�[�W�܂��̓t���[�����Ɉړ��ł���󔒃y�[�W�B
    /// </summary>
    public sealed partial class MainPage : Page
    {
        private MainPageViewModel ViewModel => this.DataContext as MainPageViewModel;

        public MainPage()
        {
            this.InitializeComponent();
        }
    }
}
```

Input��Output��CreateOutputCommand���o�C���h�����ʂ����܂��B

```xml
<Page x:Class="HelloWorldApp.Views.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:HelloWorldApp.Views"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:mvvm="using:Prism.Windows.Mvvm"
      mvvm:ViewModelLocator.AutoWireViewModel="True"
      mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBox Text="{x:Bind ViewModel.Input, Mode=TwoWay}" />
        <Button Content="���b�Z�[�W�쐬"
                Command="{x:Bind ViewModel.CreateOutputCommand}" />
        <TextBlock Text="{x:Bind ViewModel.Output, Mode=OneWay}"
                   Style="{StaticResource BodyTextBlockStyle}" />
    </StackPanel>
</Page>
```

## ���s���ē���m�F

���s����ƈȉ��̂悤�ɂȂ�܂��BTextBox�ɉ��������ă{�^���������Ɖ����Ƀ��b�Z�[�W���o�͂���܂��B

![Hello world](Images/1.png)
