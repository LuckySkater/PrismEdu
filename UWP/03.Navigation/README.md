# ��ʑJ��

Prism�̉�ʑJ�ڂ́AViewModel�N���X��INavigationService�C���^�[�t�F�[�X���g�p���čs���܂��B
INavigationService�́APrismUnityApplication�ɒ�`����Ă��邽�߁AViewModel�N���X�ɂ�DI���Ďg���K�v������܂��B
�����ł́A�{�^�������������ʑJ�ڂ���ȒP�ȃA�v���P�[�V�������쐬�������Ǝv���܂��B

## �v���W�F�N�g�̍쐬

NavigationApp�Ƃ������O��UWP�A�v�������Prism.Unity��NuGet����Q�Ƃɒǉ����܂��B
App�N���X���ȉ��̂悤�ɕҏW���ď�����Ԃ�MainPage�ɉ�ʑJ�ڂ���悤�ɂ��܂��B

```cs
using Prism.Unity.Windows;
using System.Threading.Tasks;
using Windows.ApplicationModel.Activation;

namespace NavigationApp
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

App.xaml������{�N���X��PrismUnityApplication�ɂȂ�悤�Ƀ^�O�����������܂��B

```xml
<prism:PrismUnityApplication x:Class="NavigationApp.App"
                             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                             xmlns:local="using:NavigationApp"
                             xmlns:prism="using:Prism.Unity.Windows"
                             RequestedTheme="Light">

</prism:PrismUnityApplication>
```

## MainPage�̍쐬

Views/MainPage.xaml���쐬���܂��B
��ʂɂ�MainPage�Ƃ������Ƃ��킩�邽�߂�TextBlock�Ɖ�ʑJ�ڂ̌_�@�ƂȂ�Button��u���Ă܂��B

```xml
<Page x:Class="NavigationApp.Views.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:NavigationApp.Views"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:mvvm="using:Prism.Windows.Mvvm"
      mvvm:ViewModelLocator.AutoWireViewModel="True"
      mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="MainPage"
                   Style="{StaticResource HeaderTextBlockStyle}" />
        <Button Content="��ʑJ��" />
    </StackPanel>
</Page>
```

AutoWIredViewModel�̒�`���Y�ꂸ�ɒǉ����܂��B����ViewModels/MainPageViewModel���쐬���܂��B

Prism�ł�ViewModel��ViewModelBase�N���X���p�����č쐬���܂��B
ViewModelBase�N���X�ɂ́A��ʑJ�ڂ��Ă����Ƃ��ɌĂ΂��OnNavigatedTo���\�b�h�Ɖ�ʂ��痣���Ƃ��ɌĂ΂��OnNavigationgFrom���\�b�h����`����Ă��܂��B
���ꂼ��̃��\�b�h���I�[�o�[���C�h���邱�ƂŁA��ʑJ�ڎ��̏�����ViewModel�ŋL�q�ł��܂��B
Prism���g��Ȃ��ꍇ�́AView���ł����o���Ȃ����������ɂȂ�܂��B

��ʑJ�ڏ����́AINavigationService���g���čs���܂��B
Unity���g����Prism�ł́AViewModel�̃R���X�g���N�^��INavigationService���󂯎��悤�ɂ����ViewModel�̃C���X�^���X���̃^�C�~���O�Ŏ����I��INavigationService���n����܂��B
���̃C���X�^���X���A�t�B�[���h�Ȃ�v���p�e�B�ɕێ����Ă����ĉ�ʑJ�ڂɎg���܂��B

�����ł́ANavigateNextPage���\�b�h�Ŏg�p���Ă��܂��B

```cs
using Prism.Windows.Mvvm;
using Prism.Windows.Navigation;
using System.Collections.Generic;
using System.Diagnostics;

namespace NavigationApp.ViewModels
{
    class MainPageViewModel : ViewModelBase
    {
        private INavigationService NavigationService { get; }

        // �R���X�g���N�^���`���邱�Ƃ�Unity����C���X�^���X�������œn�����
        public MainPageViewModel(INavigationService navigationService)
        {
            // Unity����n���ꂽ�C���X�^���X��ێ�
            this.NavigationService = navigationService;
        }

        // INavigationService���g���ĉ�ʑJ�ڂ��s��
        public void NavigateNextPage()
        {
            this.NavigationService.Navigate("Next", null);
        }

        public override void OnNavigatedTo(NavigatedToEventArgs e, Dictionary<string, object> viewModelState)
        {
            base.OnNavigatedTo(e, viewModelState);
            Debug.WriteLine("MainPage�ɗ��܂���");
        }

        public override void OnNavigatingFrom(NavigatingFromEventArgs e, Dictionary<string, object> viewModelState, bool suspending)
        {
            base.OnNavigatingFrom(e, viewModelState, suspending);
            Debug.WriteLine("MainPage���痣��܂�");
        }
    }
}
```

MainPage��XAML����NavigateNextPage���Ăяo���悤�ɂ��܂��B
�܂��A�R���p�C�����f�[�^�o�C���f�B���O�̂��߂̃v���p�e�B��MainPage.xaml.cs�ɒ�`���܂��B
DataContext���L���X�g���ĕԂ��Ă��邾���ɂȂ�܂��B

```cs
using NavigationApp.ViewModels;
using Windows.UI.Xaml.Controls;

namespace NavigationApp.Views
{
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

�����āAButton��Click�C�x���g��NavigateNextPage���\�b�h���o�C���h���܂��B

```xml
<Page x:Class="NavigationApp.Views.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:NavigationApp.Views"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:mvvm="using:Prism.Windows.Mvvm"
      mvvm:ViewModelLocator.AutoWireViewModel="True"
      mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="MainPage"
                   Style="{StaticResource HeaderTextBlockStyle}" />
        <Button Content="��ʑJ��" 
                Click="{x:Bind ViewModel.NavigateNextPage}"/>
    </StackPanel>
</Page>
```

## NextPage�̍쐬

���ɑ@�ې��NextPage���쐬���܂��B�V���v����TextBlock��u���������̉�ʂł��B

```xml
<Page x:Class="NavigationApp.Views.NextPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:NavigationApp.Views"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="NextPage"
                   Style="{StaticResource HeaderTextBlockStyle}" />
    </Grid>
</Page>
```


## ���s���ē���m�F

���s����ƁAMainPage���\������A�{�^����������NextPage���\������܂��B
���̂Ƃ��f�o�b�O�o�͂Ɉȉ��̂悤�ȃ��b�Z�[�W���\������邱�Ƃ��m�F�ł��܂��B�iMainPageViewModel�Ŏd���񂾃��O�j


```
MainPage�ɗ��܂���
MainPage���痣��܂�
```

![1.png](Images/1.png)

![2.png](Images/2.png)
