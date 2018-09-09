# 1.2ģ�龲̬����

�ܵ������ǣ�  
-    ����׶Σ����ӽű����� .initcall6.init �Σ�module_init ���ģ���ʼ������ xxx_init ������Ϊ.initcall6.init������ʱ���� .initcall6.init �Ρ�  
-    ���н׶Σ�start_kernel������������� .initcall6.init ����� xxx_init��  


## 1.2.1 ���ӽű����� .initcall6.init ��

�������ļ����� INIT_DATA_SECTION(16) �����˸������͵ĶΣ����а��� .initcall6.init�Σ��������£�
```
/* linux\arch\x86\kernel\vmlinux.lds.S */
	INIT_DATA_SECTION(16)
```

INIT_DATA_SECTION(16) �Ǹ��꣬���������£�
```
INIT_DATA_SECTION(16)
	INIT_CALLS
		INIT_CALLS_LEVEL(6)
			KEEP(*(.initcall6.init))
```

���ھ���չ������꣺
```
/* linux\include\linux\export.h */
#define __VMLINUX_SYMBOL(x) _##x
#define VMLINUX_SYMBOL(x) __VMLINUX_SYMBOL(x)				
````
���� __VMLINUX_SYMBOL(x) ���� x������ VMLINUX_SYMBOL(x) ���� x��

```	
/* linux\include\asm-generic\vmlinux.lds.h */
#define INIT_CALLS_LEVEL(level)						\
		VMLINUX_SYMBOL(__initcall##level##_start) = .;		\
		KEEP(*(.initcall##level##.init))			\
		KEEP(*(.initcall##level##s.init))			\
```
����INIT_CALLS_LEVEL(x)���ڣ�
```
		__initcall_x_start = .;
		KEEP(*(.initcallx.init))	
		KEEP(*(.initcallxs.init))
```

```
/* linux\include\asm-generic\vmlinux.lds.h */
#define INIT_CALLS							\
		VMLINUX_SYMBOL(__initcall_start) = .;			\
		KEEP(*(.initcallearly.init))				\
		INIT_CALLS_LEVEL(6)					\
		VMLINUX_SYMBOL(__initcall_end) = .;
```
���� INIT_CALLS ���ڣ�
```					
		__initcall_6_start) = .;
		KEEP(*(.initcall6.init))	
		KEEP(*(.initcall6s.init))				
		__initcall_end = .;
```


```		
/* linux\include\asm-generic\vmlinux.lds.h */
#define INIT_DATA_SECTION(initsetup_align)				\		
	.init.data : AT(ADDR(.init.data) - LOAD_OFFSET) {		\
		INIT_CALLS						\			
	}

```
```
/* linux\arch\x86\kernel\vmlinux.lds.S */
	INIT_DATA_SECTION(16)
```
���� INIT_DATA_SECTION(16) ���ڣ�
```
	.init.data : {		
		__initcall_6_start) = .;
		KEEP(*(.initcall6.init))	
		KEEP(*(.initcall6s.init))				
		__initcall_end = .;
	}
```
�ⶨ����.init.data�Σ�����ʱ�� .initcall6.init �� .initcall6s.init ���Ե����ݶ��������

## 1.2.2 module_init() ����ģ���ʼ����������Ϊ .initcall6.init ��

����������֪__define_initcall(xxx, 6) �ᱻչ���ɣ�
static initcall_t __initcall_xxx6 __used __attribute__((__section__(.initcall6.init))) = xxx

```
/* linux\include\linux\module.h */
	#define __define_initcall(fn, id) \
		static initcall_t __initcall_##fn##id __used \
		__attribute__((__section__(".initcall" #id ".init"))) = fn	
```
������Ĵ����֪module_init(xxx)���յ�����__define_initcall(xxx, 6)
��ˣ���Ҳ������static initcall_t __initcall_xxx6 __used __attribute__((__section__(.initcall6.init))) = xxx��
```
/* linux\include\linux\module.h */
#define module_init(x)  __initcall(x);
	#define __initcall(fn) device_initcall(fn)
		#define device_initcall(fn)     __define_initcall(fn, 6)
```


## 1.2.3 �ں˳�ʼ��ʱ����ģ���ʼ������

�ں˳�ʼ��ʱ���� initcall_t ���͵� __initcall_xxx6 ������Ҳ����ģ���ʼ�����������������£�
```
/* linux\init\main.c */
	start_kernel
		rest_init
			kernel_thread
				kernel_init
					kernel_init_freeable
						do_basic_setup
							do_initcalls
								do_initcall_level(level)
									do_one_initcall(initcall_t fn)
```