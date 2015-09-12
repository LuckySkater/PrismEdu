# ���[�U�[�Ƃ̑Θb����������InteractionRequest

MVVM�p�^�[���Ńv���O������g��ł���ƁA�悭�T���̂����[�U�[�Ƃ̑Θb�@�\�i�_�C�A���O�Ȃǁj���ǂ̂悤�Ɏ������邩�Ƃ����_�ł��B
����؂��ă_�C�A���O���o���R�[�h��ViewModel�ɏ�������AMessenger�p�^�[���Ȃǂ��g���P�[�X���悭�s���Ă���Ǝv���܂��B
Prism�ł́AInteractionRequest�Ƃ����N���X��Messenger�p�^�[�����������Ă��܂��B

InteractionRequest�N���X�͌^������1�Ƃ�AINotification�C���^�[�t�F�[�X�����������N���X���󂯎�邱�Ƃ��ł��܂��B
INotification�C���^�[�t�F�[�X�̎����N���X�Ƃ���Notification�N���X��Confirmation�N���X������܂��B
Notification�N���X���A���[�U�[�ւ̒ʒm���s���̂Ɏg�p����N���X�ŁAConfirmation�N���X�����[�U�[��Yes/No��₤�Ƃ��Ɏg���N���X�ɂȂ�܂��B

InteractionRequest�N���X���g�������[�U�[�Ƃ̑Θb�̏����̗���͈ȉ��̂悤�ɂȂ�܂��B

- InteractionRequest��Raise���\�b�h(or RaiseAsync���\�b�h)���Ă΂��
- InteractionRequest��Raise���\�b�h�̈����ɓn���ꂽNotification�I�u�W�F�N�g��View�֓n��
- View�ł�InteractionRequestTrigger�r�w�C�r�A���g���Ă��̃��b�Z�[�W���󂯎��
- PopupWindowAction��Notification���b�Z�[�W����������
	- WindowContent���ݒ肳��Ă���Ȃ�A������g����Window���o���B
	- Notification�N���X�Ȃ�A�f�t�H���g�̃_�C�A���O���o��
	- Confirmation�N���X�Ȃ�A�f�t�H���g�̊m�F�_�C�A���O���o��

## Notification�N���X���g�����ȒP�Ȓʒm

�܂��A��Ԋ�{�I��Notification�N���X���g�����_�C�A���O���o��������������܂��B
ViewModel�ɁANotification�N���X���^�����Ɏw�肵��InteractionRequest�N���X���v���p�e�B�Ƃ��Ē�`���܂��B

```cs
public InteractionRequest<Notification> NotificationRequest { get; } = new InteractionRequest<Notification>();
```

�����āACommand�̏����Ȃǂ̃_�C�A���O���o���������ňȉ��̂悤��Raise���\�b�h���Ăяo���܂��B
Notification�N���X�ɂ�Title�v���p�e�B��Content�v���p�e�B������̂Œl��ݒ肵�܂��B
Title��Window�̃^�C�g���ɐݒ肳��āAContent���_�C�A���O�̃��b�Z�[�W�ɂȂ�܂��B

```cs
this.NotificationRequest.Raise(new Notification { Title = "Alert", Content = "Notification message." });
```

View���ł́A�ȉ��̂悤��Behavior���`���܂��B

```xml
<prism:InteractionRequestTrigger SourceObject="{Binding NotificationRequest, Mode=OneWay}">
    <prism:PopupWindowAction IsModal="True"
                             CenterOverAssociatedObject="True">
        <prism:PopupWindowAction.WindowStyle>
            <Style TargetType="Window">
                <Setter Property="ResizeMode" Value="NoResize" />
                <Setter Property="SizeToContent" Value="WidthAndHeight" />
            </Style>
        </prism:PopupWindowAction.WindowStyle>
    </prism:PopupWindowAction>
</prism:InteractionRequestTrigger>
```

PopupWindowAction��Window��\������Action�ŁAIsModal�Ń��[�_���\���̐���ACenterOverAssoiatedObject�ŕ\���ʒu�̃J�X�^�}�C�Y���o���܂��B
�܂��AWindowStyle�v���p�e�B��Window��Style���ׂ�������o���܂��B
�����ł́A�ő剻�E�ŏ����{�^�����\���ɂ�����AWindow�̃T�C�Y���R���e���c�ɍ��킹���傫���ɂ���悤�ɐݒ肵�Ă��܂��B

���̃v���O���������s����ƁA�ȉ��̂悤�ȃ_�C�A���O���\������܂��B

![Notification](Images/Notification.png)

## Confirmation�N���X���g�������[�U�[�Ƃ̑Θb

����I��ViewModel����_�C�A���O�Œʒm�����邾���ł͂Ȃ�Yes/No�Ȃǂ̑I�������[�U�[�ɂ��Ăق����P�[�X������܂��B
����ȂƂ��ɂ́AConfirmation�N���X���g�p���܂��B
Confirmation�N���X�́ANotification�N���X���g�������N���X�ŁAbool�^��Confirmed�v���p�e�B�������Ă��܂��B
���̃N���X���g���ă��[�U�[�Ƃ̑Θb������ɂ͈ȉ��̂悤�ɏ����܂��B

�܂��AConfirmation���w�肵��InteractionRequest�̃v���p�e�B���`���܂��B

```cs
public InteractionRequest<Confirmation> ConfirmationRequest { get; } = new InteractionRequest<Confirmation>();
```

���ɁACommand�̏������Ȃǂ̃��[�U�[�Ɋm�F����肽���Ƃ���ŁA�ȉ��̂悤��RaiseAsync���\�b�h���Ăяo���܂��B
await�ŁA���[�U�[���Θb����������܂ő҂��܂��B
RaiseAsync���\�b�h�̖߂�l�́A���ʂ��i�[���ꂽConfirmation�I�u�W�F�N�g�ɂȂ�܂��B

```cs
private async void ConfirmationCommandExecute()
{
    var result = await this.ConfirmationRequest.RaiseAsync(new Confirmation { Title = "Confirm", Content = "OK?" });
    this.ConfirmationMessage = result.Confirmed + " selected";
}
```

���ʂ��g���đ����̏������L�q�ł��܂��B�����ł́A������ConfirmationMessage�Ƃ����v���p�e�B�ɉ����I�����ꂽ���ݒ肵�Ă��܂��B

View���ł́A�ȉ��̂悤��Behavior���`���܂��B

```xml
<prism:InteractionRequestTrigger SourceObject="{Binding ConfirmationRequest, Mode=OneWay}">
    <prism:PopupWindowAction IsModal="True"
                             CenterOverAssociatedObject="True">
        <prism:PopupWindowAction.WindowStyle>
            <Style TargetType="Window">
                <Setter Property="ResizeMode"
                        Value="NoResize" />
                <Setter Property="SizeToContent"
                        Value="WidthAndHeight" />
            </Style>
        </prism:PopupWindowAction.WindowStyle>
    </prism:PopupWindowAction>
</prism:InteractionRequestTrigger>
```

��{�I��Notification�̂Ƃ��Ɠ����ł��B
���̃v���O���������s����ƈȉ��̂悤�ȃ_�C�A���O���\������܂��B

![Confirmation](Images/Confirmation.png)

OK�{�^���������ƁhTrue selected"�ƃv���p�e�B�ɒl���ݒ肳��ACancel�{�^���������ƁhFalse selected"�ƃv���p�e�B�ɒl���ݒ肳��܂��B

## �J�X�^��Window�̕\��

Notification�I�u�W�F�N�g���g�����āAPopupWindowAction��WindowContent�v���p�e�B�Ƒg�݂��킹�邱�ƂŁA�Ǝ��̃_�C�A���O���o�����Ƃ��o���܂��B
��Ƃ��āA���������͂���InputText�_�C�A���O���쐬���Ă݂܂��B

Notification�N���X���g�����āA���͂�����������i�[���邽�߂�InputText�v���p�e�B���������N���X���`���܂��B

```cs
using Prism.Interactivity.InteractionRequest;

namespace InteractionRequestSampleModule.Models
{
    class InputNotification : Notification
    {
        public string InputText { get; set; }
    }
}
```

���ɁA���̃N���X��\�����邽�߂̉�ʂ�ViewModel���쐬���܂��B����ViewModel�N���X�ɂ�IInteractionRequestAware�C���^�[�t�F�[�X���������܂��B
���̃C���^�[�t�F�[�X�́AFinishInteraction�Ƃ���Window����ă��[�U�[�Ƃ̑Θb���I��点�邽�߂̏������󂯕t����Action�^�̃v���p�e�B��
Notification�Ƃ����ARaiseAsync���\�b�h�ɓn���ꂽNotification�I�u�W�F�N�g���󂯎�邽�߂�INotification�^�̃v���p�e�B����`����Ă��܂��B

�����̃v���p�e�B���g����OKCommand�����s���ꂽ�Ƃ���InputNotification�ɓ��̓e�L�X�g�𔽉f���鏈���������ƈȉ��̂悤�ɂȂ�܂��B

```cs
using InteractionRequestSampleModule.Models;
using Prism.Commands;
using Prism.Interactivity.InteractionRequest;
using Prism.Mvvm;
using System;

namespace InteractionRequestSampleModule.ViewModels
{
    class InputViewModel : BindableBase, IInteractionRequestAware
    {
        // IInteractionRequestAware
        public Action FinishInteraction { get; set; }
        private INotification notification;
        public INotification Notification
        {
            get { return this.notification; }
            set
            {
                this.notification = value;
                // initialize
                this.InputText = "";
            }
        }

        private string inputText;

        public string InputText
        {
            get { return this.inputText; }
            set { this.SetProperty(ref this.inputText, value); }
        }

        public DelegateCommand OKCommand { get; }

        public InputViewModel()
        {
            this.OKCommand = new DelegateCommand(() =>
                {
                    ((InputNotification)this.Notification).InputText = this.InputText;
                    this.FinishInteraction();
                },
                () => !string.IsNullOrWhiteSpace(this.InputText))
                .ObservesProperty(() => this.InputText);
        }
    }
}
```

����InputViewModel�̒��ӓ_�Ƃ��ẮA�_�C�A���O�̕\�����ɃC���X�^���X���g���܂킳��邽�߁ANotification��set���ꂽ�^�C�~���O�ŏ��������s���K�v������_�ł��B
�����ł́AInputText�v���p�e�B����ɏ��������Ă��܂��B

���ɁAView���`���܂��B����͒P����TextBox��Button����������ʂɂȂ�܂��B

```xml
<UserControl x:Class="InteractionRequestSampleModule.Views.InputView"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:local="clr-namespace:InteractionRequestSampleModule.Views"
             xmlns:prism="http://prismlibrary.com/"
             mc:Ignorable="d"
             prism:ViewModelLocator.AutoWireViewModel="True"
             d:DesignHeight="300"
             d:DesignWidth="300">
    <StackPanel>
        <Label x:Name="label"
               Content="Input text" />
        <TextBox x:Name="textBox"
                 Text="{Binding InputText, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
        <Button x:Name="button"
                Content="OK" 
                Command="{Binding OKCommand}"/>
    </StackPanel>
</UserControl>
```

���̉�ʂ�\������ɂ͈ȉ��̂悤�ɂ��܂��B

�܂��AInputNotification���^�����Ɏw�肵��InteractionRequest�^�̃v���p�e�B���`���܂��B

```cs
public InteractionRequest<InputNotification> InputNotificationRequest { get; } = new InteractionRequest<InputNotification>();
```

���ɁACommand�Ȃǂ̉�ʂ�\�������������̂Ƃ����RaiseAsync��InputNotification��n���ČĂяo���܂��B

```cs
private async void InputNotificationExecute()
{
    var result = await this.InputNotificationRequest.RaiseAsync(new InputNotification { Title = "Input" });
    this.OutputText = $"Input text is {result.InputText}.";
}
```

Confirmation�Ɠ����悤��await�Ń��[�U�[�Ƃ̑Θb�̏I����҂��܂��B���[�U�[�Ƃ̑Θb���I�������A�����ł͓��͂��ꂽ�e�L�X�g�����H����
OutputText�v���p�e�B�ɐݒ肵�Ă��܂��B

View����Behavior�̒�`�͈ȉ��̂悤�ɂȂ�܂��B

```xml
<prism:InteractionRequestTrigger SourceObject="{Binding InputNotificationRequest, Mode=OneWay}">
    <prism:PopupWindowAction>
        <prism:PopupWindowAction.WindowContent>
            <local:InputView />
        </prism:PopupWindowAction.WindowContent>
    </prism:PopupWindowAction>
</prism:InteractionRequestTrigger>
```

WindowContent�ɐ�قǍ쐬����InputView���w�肵�Ă���_���|�C���g�ł��B�������邱�Ƃŕ\�������Window�̓��e��ς��邱�Ƃ��ł��܂��B
���s����ƈȉ��̂悤�ɉ�ʂ��\������܂��B

![CutomNotification](Images/CustomNotification.png)

