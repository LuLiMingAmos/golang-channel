package main

import (
	"fmt"
	"runtime"
	"strconv"
	"sync"
)

type Int1 struct {
	Num1 chan<- Int2
}

type Int2 struct {
	Num2 int
}

var workers = runtime.NumCPU()

func doJob(inter2 chan Int2, num int, wg *sync.WaitGroup) {
	for i2 := range inter2 {
		fmt.Println("--------thread" + strconv.Itoa(num) + " " + strconv.Itoa(i2.Num2))
	}
	wg.Done()
}

func int2_insert(inter2 chan<- Int2, Len int) {
	// fmt.Println("------2")
	int2 := Int2{}
	for i := 0; i <= Len; i++ {
		int2.Num2 = i
		// fmt.Println("--------int2_insert")
		// fmt.Println(i)
		inter2 <- int2
	}

	close(inter2)
}

// func int1_insert(inter1 chan<- Int1, inter2s chan<- Int2, Len int) {
// 	fmt.Println("------1")
// 	int2 := Int2{}
// 	for inter2 := range inter2s {
// 		int2 <- inter2
// 		fmt.Println(int2.Num2)
// 	}
// 	close(inter1)
// }

func main() {
	wg := &sync.WaitGroup{}
	wg.Add(workers)
	// int1 := make(chan Int1, 100)
	int2s := make(chan Int2, 100)
	// fmt.Print(cap(int2s))
	go int2_insert(int2s, cap(int2s))
	for i := 1; i <= workers; i++ {
		go doJob(int2s, i, wg)
	}
	wg.Wait()
	// go int1_insert(int1, int2, cap(int2))
	//Do(int1, int2)
	// time.Sleep(1000 * time.Millisecond)
}
