# fixes-crutches


# Запис аудіо/відео
Коли повнорно записувати аудіо/відео, то буде зберігатись загальна тривалість останнього запису.
Щоб уникнути цього, порібно сетити OnRecordingStatusUpdate після початку запису startAsync.
Наприклад:

```js
const recording = new Audio.Recording();
await recording.prepareToRecordAsync(recordAudioConstants.highQuality);
await recording.startAsync();
recording.setOnRecordingStatusUpdate(props.recordingCallback);
recording.setProgressUpdateInterval(200);
```

# Запис аудіо в iOS та Android
Якшо використовувати стандартні пресети для запису, наприклад RECORDING_OPTIONS_PRESET_HIGH_QUALITY,
то записаний файл на iOS буде маи розширення .caf, а на Android .m4a, якшо файл повинен зберігатися на сервері
і програвтися на обох платформах то в даному випадку на Android файл не запуститься, через непітримку формату
Можна використати наступні опції для запису у високій якості з розширенням .m4a:

```js
export const highQuality = {
  android: {
    extension: '.m4a',
    outputFormat: Audio.RECORDING_OPTION_ANDROID_OUTPUT_FORMAT_MPEG_4,
    audioEncoder: Audio.RECORDING_OPTION_ANDROID_AUDIO_ENCODER_AAC,
    sampleRate: 44100,
    numberOfChannels: 2,
    bitRate: 128000,
  },
  ios: {
    extension: '.m4a',
    outputFormat: Audio.RECORDING_OPTION_IOS_OUTPUT_FORMAT_MPEG4AAC,
    audioQuality: Audio.RECORDING_OPTION_IOS_AUDIO_QUALITY_MAX,
    sampleRate: 44100,
    numberOfChannels: 2,
    bitRate: 128000,
    linearPCMBitDepth: 16,
    linearPCMIsBigEndian: false,
    linearPCMIsFloat: false,
  },
};
```

# borderless для touchableNativeFeedback
borderless не може не працювати і не знати де йому рендиретись, якшо для батьківського компонента
не задано backgroundColor, або borderRadius
щоб borderless почав працювати, можна до батьківського кмпонента додати радіус 0

```js
borderRadius: 0,
```

# touchableNativeFeedback і Text
якщо Text огорнути лиише в touchableNativeFeedback то ефекту натискання не буде,
Text можна огорнути в View після цього в touchableNativeFeedback.
Коли в touchableNativeFeedback натискати на текст, то Ripple рендериться не в місці дотику,
щоб рендер відбувався в правильному місці, можна до View задати pointerEvents="box-only":

```js
<View pointerEvents="box-only">
  <Text style={s.todayButton}> {todayLabel} </Text>
</View>
```

# FlatList
якщо Ви використовуєте FlatList то рекомендується використовувати pure компоненти для елементів в FlatList

# FlatList і клавіатура
якщо є FlatList і textInput для фільтрації(пошуку) елементів, і якшо при зміні даних
у FlatList автоматично згортається клавіатура, то можна задати пропсу keyboardDismissMode
для FlatList:

```js
keyboardDismissMode={!isLoading ? 'on-drag' : 'none'}
```
