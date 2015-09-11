# ViewModelLocator���g����

Prism�ł́AView��ViewModel��R�Â��邽�߂̋@�\�Ƃ���ViewModelLocator�Ƃ������̂�񋟂��Ă��܂��B���̋@�\��L���ɂ���ɂ�Window��UserControl�Ȃǂ�View�Ɉȉ���XAML��ǉ����邾���ł��B

```xml
xmlns:prism="http://prismlibrary.com/"
prism:ViewModelLocator.AutoWireViewModel="True"
```

���̐錾��View��XAML�ɒǉ�����ƈȉ��̂悤�ȃ��[����ViewModel�����肳��܂��B

HogeProject.FooNamespace.Views.SampleWindow�Ƃ���View���������ꍇ�AHogeProject.FooNamespace.ViewModels.SampleWindowViewModel�Ƃ������O��ViewModel�������I��DataContext�Ɋ��蓖�Ă��܂��B�iSampleView�̂悤��View�Ŗ��O���I���ꍇ�́ASampleViewModel�ɂȂ�܂��j

Bootstrapper�̎g�������ō�����悤��Views���O��Ԃ�Shell�N���X������Ă���P�[�X�ɂ��čl���Ă݂܂��BShell�ɑ΂��ď�L��XAML��ǉ������ViewModels���O��Ԃ�ShellViewModel��DataContext�ɐݒ肳��܂��B�ȉ��̂悤��ViewModel���`���Ă݂܂����B

```cs
namespace ViewModelLocatorSampleApp.ViewModels
{
    class ShellViewModel
    {
        public string Message { get; } = "Hello world";
    }
}
```

Shell�̂ق��ŁAMessage�v���p�e�B���o�C���h����悤��TextBlock��ݒu���܂��B

```xml
<Window x:Class="ViewModelLocatorSampleApp.Views.Shell"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:ViewModelLocatorSampleApp.Views"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        mc:Ignorable="d"
        Title="Shell" Height="300" Width="300">
    <Grid>
        <TextBlock Text="{Binding Message}" />
    </Grid>
</Window>
```

���s����ƈȉ��̂悤�ɂȂ�܂��B

![Shell](Images/shell.png)

���̂Ƃ��AShellViewModel�͎����I��Unity�R���e�i����擾����Ă��܂��B���̂��߁A�ȉ��̂悤�ɂ��邱�ƂŁA�I�u�W�F�N�g���O������C���W�F�N�V�������邱�Ƃ��o���܂��B

```cs
namespace ViewModelLocatorSampleApp.Models
{
    class MessageProvider
    {
        public string Message { get; } = "Hello World";
    }
}
```

���̃N���X���C���W�F�N�V��������悤��ViewModel�����������܂��B

```cs
using Microsoft.Practices.Unity;
using ViewModelLocatorSampleApp.Models;

namespace ViewModelLocatorSampleApp.ViewModels
{
    class ShellViewModel
    {
        [Dependency]
        public MessageProvider MessageProvider { get; set; }
    }
}
```

XAML��Binding��MessageProvider���g���悤�ɏ��������܂��B

```xml
<Window x:Class="ViewModelLocatorSampleApp.Views.Shell"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:ViewModelLocatorSampleApp.Views"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        mc:Ignorable="d"
        Title="Shell" Height="300" Width="300">
    <Grid>
        <TextBlock Text="{Binding MessageProvider.Message}" />
    </Grid>
</Window>
```

���s���ʂ͐�قǂƕς��Ȃ����ߊ������܂��B���̂悤��View��ViewModel�̕R�Â��ƁAViewModel�ւ̊O���N���X�̒����Ȃǂ��g����̂�Prism��Prism.Unity���g�����Ƃ��֗̕��ȓ_�ł��B