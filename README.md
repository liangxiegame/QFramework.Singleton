# QFramework.Singleton
独立的 Singleton 组件


## 使用指南


### Singleton 的调用方式:



``` csharp
xxx.Instance
```

### 如何实现一个单例?

#### 1. C# 类 通过继承 QSingleton<T>

``` csharp
namespace QFramework.Example
{
	using UnityEngine;

	class Class2Singleton :Singleton<Class2Singleton>
	{
		private static int mIndex = 0;

		private Class2Singleton() {}

		public override void OnSingletonInit()
		{
			mIndex++;
		}

		public void Log(string content)
		{
			Debug.Log("Class2Singleton" + mIndex + ":" + content);
		}
	}
	
	public class SingletonExample : MonoBehaviour
	{
		private void Start()
		{
			Class2Singleton.Instance.Log("Hello World!");
			
			// delete instance
			Class2Singleton.Instance.Dispose();
			
			// a differente instance
			Class2Singleton.Instance.Log("Hello World!");
		}
	}
}
```

#### 2. MonoBehaviour 类 通过继承 QMonoSingleton<T>

``` csharp
namespace QFramework.Example
{
	using System.Collections;
	using UnityEngine;
	
	class Class2MonoSingleton : MonoSingleton<Class2MonoSingleton>
	{
		public override void OnSingletonInit()
		{
			Debug.Log(this.name + ":" + "OnSingletonInit");
		}

		private void Awake()
		{
			Debug.Log(this.name + ":" + "Awake");
		}

		private void Start()
		{
			Debug.Log(this.name + ":" + "Start");
		}

		protected override void OnDestroy()
		{
			base.OnDestroy();
			
			Debug.Log(this.name + ":" + "OnDestroy");
		}
	}
}
```

#### 3. C# 类 通过实现静态 Instance 属性器

``` csharp
namespace QFramework.Example
{
	using UnityEngine;

	class Class2SignetonProperty : ISingleton
	{
		public static Class2SignetonProperty Instance
		{
			get { return SingletonProperty<Class2SignetonProperty>.Instance; }
		}

		private Class2SignetonProperty() {}
		
		private static int mIndex = 0;

		public void OnSingletonInit()
		{
			mIndex++;
		}

		public void Dispose()
		{
			QSingletonProperty<Class2SignetonProperty>.Dispose();
		}
		
		public void Log(string content)
		{
			Debug.Log("Class2SingletonProperty" + mIndex + ":" + content);
		}
	}
}
```

#### 4. MonoBehaivour 类 通过实现静态 Instance 属性器

``` csharp
namespace QFramework.Example
{
	using System.Collections;
	using UnityEngine;
	
	class Class2MonoSingletonProperty : MonoBehaviour,ISingleton
	{
		public static Class2MonoSingletonProperty Instance
		{
			get { return MonoSingletonProperty<Class2MonoSingletonProperty>.Instance; }
		}
		
		public void Dispose()
		{
			QMonoSingletonProperty<Class2MonoSingletonProperty>.Dispose();
		}
		
		public void OnSingletonInit()
		{
			Debug.Log(name + ":" + "OnSingletonInit");
		}

		private void Awake()
		{
			Debug.Log(name + ":" + "Awake");
		}

		private void Start()
		{
			Debug.Log(name + ":" + "Start");
		}

		protected void OnDestroy()
		{
			Debug.Log(name + ":" + "OnDestroy");
		}
	}
}
```

#### 5. 对 GameObject 进行命名

``` csharp
namespace QFramework.Example
{
    using UnityEngine;
    using QFramework;

    [MonoSingletonPath("[Example]/QMonoSingeltonPath")]
    class ClassUseMonoSingletonPath : MonoSingleton<ClassUseMonoSingletonPath>
    {
		
    }
	
    public class MonoSingletonPathExample : MonoBehaviour
    {
        private void Start()
        {
            var intance = ClassUseMonoSingletonPath.Instance;
        }
    }
}
```
结果:

![image.png](http://file.liangxiegame.com/fd3eabc2-ce61-487f-a46f-3fbc7029b8aa.png) 
