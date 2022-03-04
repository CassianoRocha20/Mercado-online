# Mercado-online
import java.util.ArrayList;
import java.util.HashMap;
import java.util.InputMismatchException;
import java.util.Map;
import java.util.Scanner;

import helper.Utils;
import model.Produto;

public class Mercado {
	private static Scanner teclado = new Scanner(System.in);
	private static ArrayList<Produto> produtos;
	private static Map<Produto, Integer> carrinho;

	public static void main(String[] args) {
		produtos = new ArrayList<Produto>();
		carrinho = new HashMap<Produto, Integer>();
		Mercado.menu();

	}

	private static void menu() {
		System.out.println("=============================");
		System.out.println("===== BEM-VINDO(A) ===== ");
		System.out.println("===== CCR20 STORE =====");
		System.out.println("==============================");

		System.out.println(" Selecione uma opçao abaixo: ");
		System.out.println(" 1 - CADASTRAR PRODUTO ");
		System.out.println(" 2 - LISTAR PRODUTOS ");
		System.out.println(" 3 - COMPRAR PRODUTO ");
		System.out.println(" 4 - VISUALIZAR CARRINHO ");
		System.out.println(" 5 - SAIR DO SISTEMA ");

		int opcao = 0;
		try {
			opcao = Integer.parseInt(Mercado.teclado.nextLine());
		} catch (InputMismatchException e) {
			Mercado.menu();
		} catch (NumberFormatException f) {
			Mercado.menu();
		}

		switch (opcao) {
		case 1:
			Mercado.cadastraProdutos();
			break;
		case 2:
			Mercado.listarProdutos();
			break;
		case 3:
			Mercado.comprarProdutos();
			break;
		case 4:
			Mercado.visualizarCarrinho();
			break;
		case 5:
			System.out.println(" VOLTE SEMPRE! ");
			Utils.pausar(2);
			System.exit(0);
		default:
			System.out.println(" OPCAO INVALIDA ");
			Utils.pausar(2);
			System.exit(0);
			Mercado.menu();
			break;
		}

	}

	private static void cadastraProdutos() {
		System.out.println(" Cadastro de Produto ... ");
		System.out.println("==============================");

		System.out.println(" Informe o nome do produto: ");
		String nome = Mercado.teclado.nextLine();

		System.out.println(" Informe o preço do produto: ");
		Double preco = Mercado.teclado.nextDouble();

		Produto produto = new Produto(nome, preco);

		Mercado.produtos.add(produto);

		System.out.println("O produto " + produto.getNome() + " foi cadastrado com sucesso. ");
		Utils.pausar(2);
		Mercado.menu();

	}

	private static void listarProdutos() {
		if (Mercado.produtos.size() > 0) {
			System.out.println("Listagem de produtos");
			System.out.println();

			for (Produto p : Mercado.produtos) {
				System.out.println(p);
				System.out.println();

			}
		} else {
			System.out.println(" Ainda nao existe produtos cadastrados. ");
		}

		Utils.pausar(2);
		Mercado.menu();

	}

	private static void comprarProdutos() {
		if (Mercado.produtos.size() > 0) {
			System.out.println(" Informe o Codigo do Produto que deseja comprar: ");
			System.out.println();

			System.out.println("======== Produtos Disponiveis ======== ");
			for (Produto p : Mercado.produtos) {
				System.out.println(p);
				System.out.println("-------------------------");

			}
			int codigo = Integer.parseInt(Mercado.teclado.nextLine());
			boolean tem = false;

			for (Produto p : Mercado.produtos) {
				if (p.getCodigo() == codigo) {
					int quant = 0;
					try {
						quant = Mercado.carrinho.get(p);
						Mercado.carrinho.put(p, quant + 1);

					} catch (NullPointerException e) {
						Mercado.carrinho.put(p, 1);
					}

					System.out.println("O Produto " + p.getNome() + " Foi adicionado ao carrinho. ");
					tem = true;

				}
				if (tem) {
					System.out.println(" Deseja adicionar outros produtos ao carrinho? ");
					System.out.println(" Informe 1 para sim ou 0 para nao");
					int op = Integer.parseInt(Mercado.teclado.nextLine());

					if (op == 1) {
						Mercado.comprarProdutos();
					} else {
						System.out.println(" Por favor, aguarde enquanto fechamos seu pedido ... ");
						Utils.pausar(2);
						Mercado.fecharPedido();
					}

				} else {
					System.out.println(" Nao foi encontrado o produto com o codigo: " + codigo);
					Utils.pausar(2);
					Mercado.menu();
				}
			}

		} else {
			System.out.println(" Ainda nao existe produto cadastrado no mercado. ");
			Utils.pausar(2);
			Mercado.menu();

		}

	}

	private static void visualizarCarrinho() {
		if (Mercado.carrinho.size() > 0) {
			System.out.println(" Produtos no Carrinho: ");
			for (Produto p : Mercado.carrinho.keySet()) {
				System.out.println("Produto " + p + "\nQuantidade: " + Mercado.carrinho.get(p));
			}

		} else {
			System.out.println("  Ainda nao existem produtos no carrinho. ");
		}

		Utils.pausar(2);
		Mercado.menu();
	}

	private static void fecharPedido() {
		Double valorTotal = 0.00;
		System.out.println(" Produtos do Carrinho ");
		System.out.println("-----------------------------");
		for (Produto p : Mercado.carrinho.keySet()) {
			int quant = Mercado.carrinho.get(p);
			valorTotal += p.getPreco() * quant;
			System.out.println(p);
			System.out.println(" Quantidade: " + quant);
			System.out.println(" -------------------------------- ");

		}
		System.out.println("Sua fatura é: " + Utils.doubleParaString(valorTotal));
		Mercado.carrinho.clear();
		System.out.println(" ----------------------- ");
		System.out.println(" Obrigado pela Preferencia ");
		System.out.println(" ----------------------- ");
		Utils.pausar(5);
		Mercado.menu();

	}

}
