---
layout: post
title: ArrayList的简单实现
tag: java ArrayList
---

```java
package demo001;

import java.util.Arrays;

public class MyArrayList<E> {
	private final int capacity = 10; // 初始容量
	private int size;
	private Object[] elementData;
	private Object[] empty = {};

	public MyArrayList() {
		this.elementData = empty;
	}

	public int size() {
		return this.size;
	}

	public boolean isEmpty() {
		return this.size == 0;
	}

	// 确定容量是否足够添加新元素或者新元素数组。
	public void ensureMiniCapacity(int miniCapacity) {
		if (miniCapacity - elementData.length > 0) {
			grow(miniCapacity);// 扩大容量
		}
	}

	private final static int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

	public void grow(int miniCapacity) {
		if (elementData.length == 0) {
			elementData = new Object[capacity];
		}
		if (miniCapacity - elementData.length > 0) {
			int oldCapacity = elementData.length;
			int newCapacity = oldCapacity + (oldCapacity >> 1);
			if (newCapacity - newCapacity < 0)
				newCapacity = miniCapacity;
			if (newCapacity - MAX_ARRAY_SIZE > 0)
				newCapacity = hugeCapacity(miniCapacity);
			elementData = Arrays.copyOf(elementData, newCapacity);
		}
	}

	private static int hugeCapacity(int minCapacity) {
		if (minCapacity < 0) // overflow
			throw new OutOfMemoryError();
		return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE
				: MAX_ARRAY_SIZE;
	}

	// 在list尾部添加一个元素
	public boolean add(E e) {
		ensureMiniCapacity(this.size + 1);
		elementData[size++] = e;
		return true;
	}

	// 在index位置添加一个元素 注意如果是在尾部请使用add,不要使用index = this.size
	public void add(int index, E e) {
		rangCheck(index);
		ensureMiniCapacity(this.size + 1);
		System.arraycopy(elementData, index, elementData, index + 1, size
				- index);
		elementData[index] = e;
		size++;
	}

	// 在list尾部添加一个数组
	public boolean addAll(E[] array) {
		ensureMiniCapacity(this.size + array.length);
		Object[] tmp = (Object[]) array;
		System.arraycopy(tmp, 0, elementData, size, array.length);
		size += array.length;
		return true;
	}

	// 在index位置添加一个数组 注意如果是在尾部请使用add,不要使用index = this.size
	public boolean addAll(int index, E[] array) {
		rangCheck(index);
		ensureMiniCapacity(this.size + array.length);
		System.arraycopy(elementData, index, elementData, index + array.length,
				size - index);
		System.arraycopy(array, 0, elementData, index, array.length);
		size += array.length;
		return true;
	}

	// 移除指定位置的元素，并且使得所有右边的元素索引减1
	@SuppressWarnings("unchecked")
	public E remove(int index) {
		rangCheck(index);
		Object removeValue = elementData[index];
		System.arraycopy(elementData, index + 1, elementData, index, size - 1
				- index);
		elementData[size - 1] = null;
		size--;
		return (E) removeValue;
	}

	// 检查是否超出索引
	private void rangCheck(int index) {
		if (index < 0 || index > this.size - 1) {
			throw new ArrayIndexOutOfBoundsException("out of list");
		}
	}

	@SuppressWarnings("unchecked")
	// 设置index位置元素
	public E set(int index, E e) {
		rangCheck(index);
		elementData[index] = e;
		return (E) elementData[index];
	}

	// 取得index位置的元素
	@SuppressWarnings("unchecked")
	public E get(int index) {
		rangCheck(index);
		return (E) elementData[index];

	}

	@SuppressWarnings("unchecked")
	// 转化为数组
	public Object[] toArray() {
		Object[] arrayCopy = new Object[size];
		System.arraycopy(elementData, 0, arrayCopy, 0, size);
		return arrayCopy;
	}

	// this is unit test
	public static void main(String args[]) {
		MyArrayList<String> list = new MyArrayList<String>();
		String[] arrayStr = { "1", "2", "3", "4" };
		list.addAll(arrayStr);
		list.add(2, "f");
		list.remove(2);
		list.addAll(3, arrayStr);
		for (Object o : list.toArray()) {
			System.out.print(o + " ");
		}
		System.out.println("\n" + list.size());
	}

}

```
