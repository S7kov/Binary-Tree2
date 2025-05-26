public class BinaryTree<T extends Comparable<T>> {
    private static class Node<T> {
        T data;
        Node<T> left;
        Node<T> right;
        
        Node(T data) {
            this.data = data;
            this.left = null;
            this.right = null;
        }
    }
    
    private Node<T> root;
    
    public BinaryTree() {
        root = null;
    }
    
    // Добавление элемента в дерево
    public void add(T data) {
        root = addRecursive(root, data);
    }
    
    private Node<T> addRecursive(Node<T> current, T data) {
        if (current == null) {
            return new Node<>(data);
        }
        
        if (data.compareTo(current.data) < 0) {
            current.left = addRecursive(current.left, data);
        } else if (data.compareTo(current.data) > 0) {
            current.right = addRecursive(current.right, data);
        }
        
        return current;
    }
    
    // Поиск элемента
    public boolean contains(T data) {
        return containsRecursive(root, data);
    }
    
    private boolean containsRecursive(Node<T> current, T data) {
        if (current == null) {
            return false;
        }
        
        if (data.equals(current.data)) {
            return true;
        }
        
        return data.compareTo(current.data) < 0 
            ? containsRecursive(current.left, data) 
            : containsRecursive(current.right, data);
    }
    
    // Удаление элемента
    public void delete(T data) {
        root = deleteRecursive(root, data);
    }
    
    private Node<T> deleteRecursive(Node<T> current, T data) {
        if (current == null) {
            return null;
        }
        
        if (data.equals(current.data)) {
            // Узел без детей
            if (current.left == null && current.right == null) {
                return null;
            }
            
            // Узел с одним ребенком
            if (current.right == null) {
                return current.left;
            }
            if (current.left == null) {
                return current.right;
            }
            
            // Узел с двумя детьми
            T smallestValue = findSmallestValue(current.right);
            current.data = smallestValue;
            current.right = deleteRecursive(current.right, smallestValue);
            return current;
        }
        
        if (data.compareTo(current.data) < 0) {
            current.left = deleteRecursive(current.left, data);
            return current;
        }
        
        current.right = deleteRecursive(current.right, data);
        return current;
    }
    
    private T findSmallestValue(Node<T> root) {
        return root.left == null ? root.data : findSmallestValue(root.left);
    }
    
    // Обход в глубину (in-order)
    public void traverseInOrder() {
        traverseInOrderRecursive(root);
    }
    
    private void traverseInOrderRecursive(Node<T> node) {
        if (node != null) {
            traverseInOrderRecursive(node.left);
            System.out.print(node.data + " ");
            traverseInOrderRecursive(node.right);
        }
    }
    
    // Обход в ширину
    public void traverseLevelOrder() {
        if (root == null) {
            return;
        }
        
        Queue<Node<T>> nodes = new LinkedList<>();
        nodes.add(root);
        
        while (!nodes.isEmpty()) {
            Node<T> node = nodes.remove();
            System.out.print(node.data + " ");
            
            if (node.left != null) {
                nodes.add(node.left);
            }
            
            if (node.right != null) {
                nodes.add(node.right);
            }
        }
    }
    
    // Проверка на пустоту
    public boolean isEmpty() {
        return root == null;
    }
    
    // Очистка дерева
    public void clear() {
        root = null;
    }
}
