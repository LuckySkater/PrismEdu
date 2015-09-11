# Bootstrap���g����

Prism�ł́A�A�v���P�[�V�����̋N���V�[�P���X���i��Bootstrap�N���X���p�ӂ���Ă��܂��B�����ł́A������g���ĉ�ʂ�\�����邾����Hello world�A�v���P�[�V�������쐬���Ă݂܂��B

## ���C�u�����̓���

HelloWorldApp�Ƃ������O�Ńv���W�F�N�g���������NuGet����ȉ���2�̃��C�u������ǉ����܂��B

- Prism.Core
- Prism.Unity

�O�҂�Prism�̊�{�I�ȃR�A�@�\�ɂȂ�܂��B��҂́APrism�Ŏg�p����DI�R���e�i�Ƃ���Unity�������ł͎g���Ă������Ǝv���Ă���̂ŁA����̂��߂̃��C�u�����ɂȂ�܂��B

## ��{�ƂȂ�Window�̍쐬

Prism�ł́A��{�ƂȂ�Window��1�K�v�ɂȂ�܂��B��ʓI�ɂ�Shell�Ƃ������O���p�����Ă���݂����ł��B�Ȃ̂ŁAShell�Ƃ������O��Window���쐬���܂��BMainWindow.xaml���폜���ĐV�K��Views���O��Ԃ�Window��Shell�Ƃ������O�ō쐬���܂��B

���ɁA�v���W�F�N�g������Bootstrapper�Ƃ������O�̃N���X���쐬���܂��B


```cs
using HelloWorldApp.Views;
using Microsoft.Practices.Unity;
using Prism.Unity;
using System.Windows;

namespace HelloWorldApp
{
    class Bootstrapper : UnityBootstrapper
    {
        protected override DependencyObject CreateShell()
        {
            // this.Container��Unity�̃R���e�i���擾�ł���̂�
            // ��������Shell���쐬����
            return this.Container.Resolve<Shell>();
        }

        protected override void InitializeShell()
        {
            // Shell��\������
            ((Window)this.Shell).Show();
        }
    }
}
```

�����܂łŁA�v���W�F�N�g�͈ȉ��̂悤�Ȋ����ɂȂ��Ă���͂��ł��B

- HelloWorldApp
	- Views
		- Shell.xaml
	- Bootstrapper.cs


�Ō�ɁABootstrapper���g���悤�ɃA�v���P�[�V�����̋N�����������������܂��BApp.xaml���ȉ��̂悤�ɏ���������Application_Startup�C�x���g�n���h�����쐬���܂��B

```xml
<Application x:Class="HelloWorldApp.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:HelloWorldApp"
             Startup="Application_Startup">
    <Application.Resources>
         
    </Application.Resources>
</Application>
```

Application_Startup�C�x���g�n���h����Bootstrapper�̋N�������������܂��B

```cs
using System.Windows;

namespace HelloWorldApp
{
    /// <summary>
    /// App.xaml �̑��ݍ�p���W�b�N
    /// </summary>
    public partial class App : Application
    {
        private void Application_Startup(object sender, StartupEventArgs e)
        {
            // Bootstrapper���N������
            new Bootstrapper().Run();
        }
    }
}
```

���̏�ԂŃA�v���P�[�V�������N������ƈȉ��̂悤��Window���\������܂��B

![Window](Images/shell.png)

�ȏオPrism���g�����ŏ��̃A�v���P�[�V�����ɂȂ�܂��B