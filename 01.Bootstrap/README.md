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




