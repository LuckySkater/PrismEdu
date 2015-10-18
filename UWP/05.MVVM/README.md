# MVVM�̊�{�N���X

Prism�ɂ�MVVM���x������N���X����������`����Ă��܂��B

- BindableBase
- DelegateCommand
- ViewModelBase
- ValidatableBindableBase

BindableBase�y�сADelegateCommand�ɂ��Ă�WPF�łƋ��ʂȂ��߂�������Q�Ƃ��Ă��������B

[MVVM�̊�{�N���X](../../04.MVVMBasic/README.md)

ViewModelBase�ɂ��ẮA��ʑJ�ڗp�̃R�[���o�b�N�@�\�ƈꎞ�ۑ��ɑΉ�����BindableBase�̔h���N���X�ł��B
��ʑJ�ڂɂ��ẮA[��ʑJ��](../03.Navigation/README.md)���Q�Ƃ��Ă��������B
�ꎞ�ۑ��ɂ��Ă͕ʋL���ŉ�����܂��B

## ValidatableBindableBase

Prism�ɂ́ADataAnnotations�ɂ����͒l�̌��؂��T�|�[�g����N���X���쐬���邽�߂�ValidatableBindableBase�Ƃ����N���X������܂��B
���̃N���X���g���ƊȒP�ɓ��͒l�̌��؋@�\����邱�Ƃ��ł��܂��B

�쐬���@�́AValidatableBindableBase�N���X���p�����āASetProperty���ĂԌ`�Ńv���p�e�B���`���邾���ł��B
���Ƃ́A���؂������v���p�e�B��DataAnnotations�����邱�Ƃŋ@�\���L��������܂��B

Input�Ƃ����v���p�e�B���K�{���͂̃N���X�̒�`����ȉ��Ɏ����܂��B

```cs
using Prism.Windows.Validation;
using System.ComponentModel.DataAnnotations;

namespace ValidatableBindableBaseApp.ViewModels
{
    class InputDataViewModel : ValidatableBindableBase
    {
        private string input;

        [Required(ErrorMessage = "���͂��Ă�������")]
        public string Input
        {
            get { return this.input; }
            set { this.SetProperty(ref this.input, value); }
        }

    }
}
```

�f�t�H���g�ł́A�v���p�e�B�ɒl���ݒ肳�ꂽ�^�C�~���O�Œl�̌��؂�����܂��B
���̂ق��ɔC�ӂ̃v���p�e�B�̒l�̌��؂�A�S�Ẵv���p�e�B�̒l�̌��؂��s�����\�b�h���񋟂���Ă��܂��B

- ValidateProperty
- ValidateProperties

���\�b�h�̖߂�l��bool�^�ŁAfalse���Ԃ�ƃG���[�����邱�Ƃ������Ă��܂��B

### �G���[�̌��ʂ̎擾

ValidatableBindableBase��Errors�v���p�e�B�Ŏ擾�ł���N���X�ɂ̓C���f�N�T����`����Ă��āA�����Ƀv���p�e�B����n�����ƂŃG���[���b�Z�[�W�̃R���N�V�������擾�ł��܂��B
�������ʂ�Binding���邱�ƂŁA�G���[���b�Z�[�W�̕\�����ł��܂��B

ViewModel��Input�Ƃ������O�Ő�قǂ̃N���X���`������ʂŃG���[���b�Z�[�W�̕\��������������@���ȉ��Ɏ����܂��B

```xml
<Page x:Class="ValidatableBindableBaseApp.Views.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:ValidatableBindableBaseApp.Views"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:mvvm="using:Prism.Windows.Mvvm"
      mvvm:ViewModelLocator.AutoWireViewModel="True"
      mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="���͍���"
                   Style="{StaticResource CaptionTextBlockStyle}" />
        <TextBox Text="{x:Bind ViewModel.InputData.Input, Mode=TwoWay}" />
        <ItemsControl ItemsSource="{Binding InputData.Errors[Input], Mode=OneWay}"
                      Foreground="Red" />
        <Button Content="�`�F�b�N"
                Click="{x:Bind ViewModel.Alert}" />
    </StackPanel>
</Page>
```

�\�����ʂ͈ȉ��̂悤�ɂȂ�܂��B

![1.png](Images/1.png)

�G���[�̗L���̔���́AGetAllErrors���\�b�h�ɗv�f���܂܂�Ă邩�ǂ����Ŕ���ł��܂��B
�R�[�h����ȉ��Ɏ����܂��B

```cs
public async void Alert()
{
    if (this.InputData.GetAllErrors().Any())
    {
        var dlg = new MessageDialog("HasError");
        await dlg.ShowAsync();
    }
    else
    {
        var dlg = new MessageDialog("NoError");
        await dlg.ShowAsync();
    }
}
```
