# -c-133-135
Реализация односвязного списка c++  #133-135
template<typename T>
class List//класс список
{
public:
	List();
	~List();
	void pop_front();//удаляем перв элемент для очистки памяти
	void push_back(T data);
	//метод, реал-я вне тела. Вставить в конец списка
	void clear();//для очистки списка
	int Getsize() { return size; }
	//чтобы обратиться к элементу, н.перегрузить [],создадим оператор
	T& operator[](const int index);//принимаемый парам индекс -'это номер того эл-та,
	// которыймы д. вернуть. Также будем возвращать ссылку на тип Т, ссылка - т к есть 
	//возможность изменять возвращаемые данные. Название оператора - []

	//добавление элемента в начало списка
	void push_front(T data);

	//добавление элемента в список по указанному индексу
	void insert(T data, int index);

	//удаление элемента в списке по указанному индексу
	void removeAt(int index);

	//удаление последнего элемента в списке
	void pop_back();
private:
	
	template<typename T>

	class Node//вложен. класс элементов, типа узел связи одного элемента с другим
	{
	public:
		Node *pNext;//указатель/адрес на след. элемент в списке
		T data;//б. харанить данные обощенного типа

		Node(T data=T(), Node *pNext=nullptr)//по умолчанию *pNext=nullptr. т к
		//если м ыб добавлять эл-т в к. списка, то он не должен содерожать ссылку на адрес
		// data=T() чтобы в поле данные не был мусор. а лежали данные любого типа, потому Т
		{
			this->data = data;
			this->pNext = pNext;
		}
};

	int size;//кол-во элементов в нашем списке

	Node <T> *head;//указатель, которая изображает первый элемент списка. Указатель,
	//т к все элементы б. выделяться в динамической памяти
	//Чтобы там был любой тип данных, надо сделать класс List шаблонным
	//инициал-т эти перем и ук-ль б. в кторе пи создании
};
//вынесли ктор и дектор за тело класса

template<typename T>//т к класс Node стал шаблонным, то при вынесении кторов надо
//тоже указать, что они шаблонные
List<T>::List()
{
	size = 0;//инициализация
	head = nullptr;
}

template<typename T>
List<T>::~List()
{
	clear();
}

template<typename T>
void List<T>::pop_front()
{
	Node <T>* temp= head;//создаем объект временный нашего Node
	//для хранения нулевого эл-та списка, чтобы мы могли его удалить

	head = head->pNext;//а в head мы присвоим адрес след-го элемента, который идет 
	//за head. наш head как бы сдвинется и станет уже вторым элементом

	delete temp;//теперь можно безнаказанно удалить бывший head
	size--;//т к огна ведет счет кол-ву элементов в списке

}

template<typename T>
void List<T>::push_back(T data)
//сначала проверим не пустой ли 1й эл-т, вдруг он еще не создан
//если его нет, т создаем новый элемент
{
	if (head==nullptr)
	{
		head = new  Node<T>(data);
	}
	else//если в поле что-то есть, оно  не пустое
		// создаем новый эл - т, присваиваем адрес этого нового  эдемента
		//самому последнему полю pNext, самому последнему адресу в нашем 
		//списке, который указывает на налл
	{
		Node <T> *current = this->head;//создаем перем, которая б присваивать 
		//указатель в каждбом узле на данный момент ей присвоили наш первый эл-т

		while (current->pNext !=nullptr)
	//мы проверяем адреса элементов по очереди,если адрес текуего элемента  не
	//равен налпоинтер, то ему присваиваем след эл-т, затем опять смотрим его адрес
		{
			current = current->pNext;
	//то в поле текущее current будем присваивать 
	//след элемент,так до тех пор пока не наткнемся на наллптр. Тогда потом мы 
	//создадим нов эл-т и присвоим вместо налл его адрес
		}
		current->pNext = new Node<T>(data);//создадим нов элемент и добавим его в поле pNext
	}
	size++;
}


template<typename T>
void List<T>::clear()
{
	while (size)
	{
		pop_front();
	}
}

template<typename T>
T & List<T>::operator[](const int index)//вынесли метод перегрузки оператора [] отдельно
{
	// TODO: вставьте здесь оператор return
	//мы заходим в  headэлемент, значеие счетчика =1. затем заходим в след,
	// счетчик =2, и как олько переданный номер элемента в параметре совпадет
	//с номером в счетчике, мы возвратим данные из этого эелемента

	int counter = 0;//наш счетчик считает в каком элементе мы находимся
	Node <T> *current = this->head;//керрент отвечает в каком элементе мы сейчас находимся

	while (current!= nullptr)//пока текущий элемент не равен нулю
	{
		if (counter==index)
		{
			return current->data;//забираем у текущего элемента данные
		}
	

	current = current->pNext;//в перем current (отвечает, какой элемент на этот момент)
		//присвоим адрес след элемента, Для этого м ыу текущей перем спросим адрес его 
		//следующего элемента, а счетчик counter увеличим на единицу

		counter++;}
}


template<typename T>
void List<T>::push_front(T data)//добавить эл-т в начало списка
{
	head = new Node<T>(data, head);//создаем эл-т, присвоить head, 
	//парам. , head) -присвоение  новому эл-ту адреса -указ-ль  старого head

	size++;
}


template<typename T>
void List<T>::insert(T data, int index)
{

	if (index == 0)
	{
		push_front(data);
	}
	else
	{
		Node<T> *previous = this->head;
		//создаем временный указ-ль, присваиваем ему значение head
		//находим в цикле эл-т с индексом, предшествующим индексу,
		//на место которого мы хотим поместить объект


		for (int i = 0; i < index - 1; i++)
		{
			previous = previous->pNext;//теперб previous
			//содержит адрес и данные предшеств-го элемента
		}

		Node<T> *newNode = new Node<T>(data, previous->pNext);
		//создаем нов объект, ему передаем данные  и указатель предыдущего объекта
		previous->pNext = newNode;//в pNext добавляем адрес текущего объекта

		size++;
	}
}

template<typename T>
void List<T>::removeAt(int index)//удалить любой элемент по указ-му индексу
{
	if (index == 0)
			{
				pop_front();
			}
			else
			{
				Node<T> *previous = this->head;//найти предыд-й эл-т, относительно того
				//который хотим удалить

				for (int i = 0; i < index - 1; i++)
				{
					previous = previous->pNext;//присвоение адреса предыдущего следующему 
					//за удаляемым
				}
		
				
				Node<T> *toDelete = previous->pNext;
				//в перем toDelete храним адрес старого эл-та, на который 
				//указывал предыдущий эл-т
		
				previous->pNext = toDelete->pNext;
		
				delete toDelete;
		
				size--;
			}

}

template<typename T>
void List<T>::pop_back()
{
	removeAt(size - 1);//аолучаем самый последний элемент в списке
}

int main()
{
	setlocale(LC_ALL, "ru");
	List<int> lst;
	lst.push_front(8);
	lst.push_front(7);
	lst.push_front(101);
	lst.push_back(5);//в итоге в data=5,pNext=nullptr по умолчанию, 
	//т к его нет на след элемент пока  создали первый элемент

	lst.push_back(10);//что-то есть в head
	lst.push_back(22);
	cout <<"Элементов в списке: "<< lst.Getsize() << endl;
	cout << lst[2] << endl;

	int numbersCount;
	/*cin >> numbersCount;
	for (int i = 0; i < numbersCount; i++)
	{
		lst.push_back(rand()%10);
	}*/
	lst.pop_front();
	
	for (int i = 0; i < lst.Getsize(); i++)
	{
		cout << lst[i] << endl;
	}

	cout << endl << "Выполняю метод insert! " << endl;
	lst.insert(99, 1);
	for (int i = 0; i < lst.Getsize(); i++)
	{
		cout << lst[i] << endl;
	}

	cout << endl << "Выполняю метод remove_at! " << endl;
	lst.removeAt(4);
	for (int i = 0; i < lst.Getsize(); i++)
	{
		cout << lst[i] << endl;
	}

	cout << endl << "Выполняю метод pop_front! " << "Количество элементов в списке "
		<< lst.Getsize() << endl;

	lst.clear();
	cout << endl << "Выполняю метод clear! " << "Количество элементов в списке осталось "
		<< lst.Getsize() << endl;



	return 0;
}



