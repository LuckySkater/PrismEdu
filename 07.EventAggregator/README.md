# ���W���[���Ԃ̒ʐM������EventAggregator

Prism�ő�K�͂ȃA�v���P�[�V������g��ł����ƁA����Module�ŃA�v���P�[�V��������邱�Ƃ�����܂��B
����Module�ŃA�v���P�[�V������g��ł���ƁAModule�ԂŃf�[�^�̂����Ȃǂ��s�������P�[�X�������܂��B
�Ⴆ�΁A�f�[�^�̓o�^������Module�������āA���ʂ�\������Module������Ƃ��܂��B
���̂Ƃ��A�f�[�^�̓o�^�����猋�ʂ�\������Ƃ��������Ƃ͂�肽���Ȃ�Ǝv���܂��B
Module�𕪂���ƊȒP�ɂ̓f�[�^�̂�����C�x���g�ɂ�錋�����s���Ȃ��Ȃ�܂��B
����ȂƂ��Ɏg����̂�IEventAggregator�ɂȂ�܂��B

IEventAggregator�́A���O�̒ʂ�C�x���g�̒���҂ł��B
Prism��UnityBootstrapper�ō쐬���ꂽIUnityContainer���ɂ́A�V���O���g����IEventAggregator�̃C���X�^���X���Ǘ�����Ă��܂��B
����IEventAggregator��Module�Ԃŋ��L���邱�ƂŃC�x���g�̂���肪�a�����ɍs����悤�ɂȂ�܂��B

## IEventAggregator�̊ȒP�Ȏg����

IEventAggregator���̂́AT GetEvent<T>()�Ƃ������\�b�h���������̃V���v���ȃC���^�[�t�F�[�X�ł��B
GetEvent�̌^�����Ŏw�肵���C�x���g�������^�̏ꍇ�͓����C���X�^���X��Ԃ��悤�ɂȂ��Ă��܂��B
GetEvent�̌^�����ɂ́A���ʂɎg���ꍇ�́APubSubEvent<TPayload>�^�̔h���N���X���g�p���܂��B
�ȉ��̂悤�ȃN���X���`���܂��B

```cs
using Prism.Events;

namespace EventAggregatorSample.Infrastructure
{
    public class InputValueEvent : PubSubEvent<InputValue>
    {
    }

    public class InputValue
    {
        public int Value { get; set; }
    }
}
```

���̃N���X���L�[�ɂ��āAModule�ԂŃC�x���g�̂��������܂��B

### �C�x���g�̔��s

���̃C�x���g�̔��s�̎d���͈ȉ��̂悤�ɂȂ�܂��B

- Unity�̃R���e�i����C���W�F�N�V�������Ă������IEventAggregator�ɑ΂��āAGetEvent�ŃC�x���g���擾
- Publish���\�b�h��InputValue��n��

�R�[�h�ɂ���ƈȉ��̂悤�ɂȂ�܂��B

```cs
using EventAggregatorSample.Infrastructure;
using Microsoft.Practices.Unity;
using Prism.Events;
using System;

namespace EventAggregatorSample.Input.Models
{
    public class ValuePublisher : IValuePublisher
    {
        [Dependency]
        public IEventAggregator EventAggregator { get; set; }

        private Random Random { get; } = new Random();
        
        public void Publish()
        {
            this.EventAggregator
                .GetEvent<InputValueEvent>()
                .Publish(new InputValue { Value = this.Random.Next(100) });
        }        
    }
}
```

0�`99�܂ł̃����_���Ȑ����𔭍s���Ă��܂��B

### �C�x���g�̍w��

�C�x���g�̎�M���́A�ȉ��̂悤�ȗ���ɂȂ�܂��B

- Unity�̃R���e�i����C���W�F�N�V�������Ă��炽IEventAggregator�ɑ΂��āAGetEvent�ŃC�x���g���擾
- Subscribe���\�b�h�ŃC�x���g��M���̏�����n���B
	- �I�v�V�����Ƃ��Ĉ����ŁA�ǂ̃X���b�h�Ŏ��s���邩���w��\
- Subscribe���\�b�h�̖߂�l�ɑ΂���Dispose���\�b�h���Ăяo�����ƂŃC�x���g�̎�M�𒆎~����

���̂Ƃ����ӂ���_�́ASubscribe�œn�����f���Q�[�g�͎�Q�ƂŊǗ�����Ă���Ƃ����_�ł��B
�t�B�[���h���Q�Ƃ��ĂȂ��悤�ȃ����_����n����GC�Ȃǂ̃^�C�~���O�ŏ��������s����Ȃ��Ȃ�܂��B
�t�B�[���h���Q�Ƃ��Ă��郉���_�����A�C���X�^���X���\�b�h�̎Q�Ƃ�n���悤�ɂ����ق����Ӑ}���Ȃ��^�C�~���O��
���������s����Ȃ��Ȃ�����Ƃ��������Ƃ��Ȃ��Ĉ��S�ł��B

��قǂ�InputValueEvent���w�ǂ���R�[�h�͈ȉ��̂悤�ɂȂ�܂��B

```cs
using EventAggregatorSample.Infrastructure;
using Prism.Events;
using Prism.Mvvm;

namespace EventAggregatorSample.Output1.Models
{
    public class SingleValueProvider : BindableBase, ISingleValueProvider
    {
        private int value;

        public int Value
        {
            get { return this.value; }
            private set { this.SetProperty(ref this.value, value); }
        }

        public SingleValueProvider(IEventAggregator eventAggregator)
        {
            eventAggregator
                .GetEvent<InputValueEvent>()
                .Subscribe(x => this.Value = x.Value, ThreadOption.UIThread);
        }
    }
}
```

ThreadOption��UI�X���b�h��Ŏ��s�����悤�Ɏw�肵�Ă��܂��B
���̑��ɂ��ABackgroundThread�i���O�̒ʂ�o�b�N�O���E���h�Ŏ��s����j��PublisherThread�i���O�̂Ƃ���C�x���g���s�҂Ɠ����X���b�h�Ŏ��s����j���w��ł��܂��B

## �T���v���v���O�����̉��

���̃t�H���_�Ɋi�[����Ă���T���v���v���O�����͈ȉ��̂悤�ȃv���W�F�N�g�ō\������Ă��܂��B

- EventAggregatorSampleApp
	- Shell��Bootstrapper���������v���W�F�N�g
	- Shell��InputRegion��OutputRegion�ɕ�����Ă�
- EventAggregatorSample.Infrastructure
	- InputValue��InputValueEvent�ȂǁA����Module�Ԃŋ��L����N���X���`���Ă���
- EventAggregatorSample.Input
	- InputValueEvent�𔭍s����Module
- EventAggregatorSample.Output1
	- InputValueEvent���w�ǂ��Ēl��\������Module
- EventAggregatorSample.Output2
	- InputValueEvent���w�ǂ��Ēl�̗�����\������Module
