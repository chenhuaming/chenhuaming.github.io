---
layout: post
title: LinkedList的简单实现
tag: java LinkedList
---

```java
package demo001;

import java.util.NoSuchElementException;

public class MyLinkedList<E> {
	private int size;

	public int size() {
		return this.size;
	}

	private Node<E> first;
	private Node<E> last;

	// 在首部添加一个元素
	public boolean addFisrt(E element) {
		linkFirst(element);
		return true;
	}

	// 同 addLast
	public boolean add(E element) {
		linkLast(element);
		return true;
	}

	// 在末尾添加一个元素
	public boolean addLast(E element) {
		linkLast(element);
		return true;
	}

	// 表示插入元素成为index,这里简单实现,index == size(使用add(E element)插入)
	public boolean add(int index, E element) {
		rangeCheck(index);
		final Node<E> x = node(index);
		final Node<E> prev = x.prev;
		final Node<E> newNode = new Node<E>(prev, element, x);
		x.prev = newNode;
		if (prev == null)
			first = newNode;
		else
			prev.next = newNode;
		size++;
		return true;
	}

	public E remove(int index) {
		rangeCheck(index);
		return unlink(node(index));
	}

	private E unlink(Node<E> x) {
		final E element = x.item;
		final Node<E> prev = x.prev;
		final Node<E> next = x.next;
		if (prev == null)
			first = next;
		else
			prev.next = next;

		if (next == null)
			last = prev;
		else
			next.prev = prev;
		size--;
		// help gc
		x.item = null;
		x.next = null;
		x.prev = null;
		x = null;
		return element;
	}

	public E get(int index) {
		rangeCheck(index);
		return node(index).item;
	}

	private Node<E> node(int index) {
		if (index <= (size >> 1)) {
			Node<E> node = first;
			for (int i = 0; i < index; i++)
				node = node.next;
			return node;
		} else {
			Node<E> node = last;
			for (int i = size - 1; i > index; i--)

				node = node.prev;
			return node;
		}
	}

	private void rangeCheck(int index) {
		if (!(index >= 0 && index < size)) {
			throw new IndexOutOfBoundsException();
		}
	}

	private void linkFirst(E element) {
		final Node<E> f = first;
		Node<E> newNode = new Node<>(null, element, f);
		first = newNode;
		if (f == null)
			last = newNode;
		else
			f.prev = newNode;
		size++;
	}

	private void linkLast(E element) {
		final Node<E> l = last;
		Node<E> newNode = new Node<>(l, element, null);
		last = newNode;
		if (l == null)
			first = newNode;
		else
			l.next = newNode;
		size++;
	}

	public E getFirst() {
		if (first == null) {
			throw new NoSuchElementException();
		}
		return first.item;
	}

	public E getLast() {
		if (last == null) {
			throw new NoSuchElementException();
		}
		return last.item;
	}

	static class Node<E> {
		E item;
		Node<E> prev;
		Node<E> next;

		Node(Node<E> prev, E element, Node<E> next) {
			this.item = element;
			this.prev = prev;
			this.next = next;
		}
	}

	public static void main(String args[]) {
		MyLinkedList<Integer> list = new MyLinkedList<>();
		for (int i = 0; i < 10; i++) {
			list.add(i);
		}
		list.remove(0);
		list.remove(7);
		list.add(0, 1000);
		list.add(8, 11111);
		for (int i = 0; i < list.size(); i++) {
			int element = list.get(i);
			System.out.println(i + " = " + element);
		}
		System.out.println(list.getFirst());
		System.out.println(list.getLast());
	}
}


```
