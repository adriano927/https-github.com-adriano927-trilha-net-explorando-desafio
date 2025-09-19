# https-github.com-adriano927-trilha-net-explorando-desafio
sistema de hospedagem simples

O desafio proposto consiste em criar um sistema de hospedagem simples utilizando Programação Orientada a Objetos (POO). A ideia é simular a reserva em um hotel, contemplando hóspedes, suítes e o cálculo do valor total da estadia. O sistema deve ainda aplicar um desconto de 10% no valor final quando a reserva ultrapassar 10 dias.

O código foi desenvolvido em C# e estruturado em três classes principais:

Pessoa → Representa o hóspede, armazenando nome e sobrenome.

Suite → Representa a suíte do hotel, contendo informações como tipo, capacidade e valor da diária.

Reserva → Responsável por vincular os hóspedes à suíte, calcular o valor total da hospedagem e garantir que a quantidade de hóspedes não ultrapasse a capacidade da suíte.

No método principal (Main), criamos alguns hóspedes, uma suíte e uma reserva de 12 dias. O programa exibe no console a quantidade de hóspedes, seus nomes, o tipo de suíte reservada e o valor final da hospedagem, já com desconto aplicado quando necessário.

using System;
using System.Collections.Generic;

namespace SistemaHospedagem
{
    // Classe Pessoa - representa o hóspede
    public class Pessoa
    {
        public string Nome { get; set; }
        public string Sobrenome { get; set; }

        public Pessoa(string nome, string sobrenome)
        {
            Nome = nome;
            Sobrenome = sobrenome;
        }

        public string NomeCompleto => $"{Nome} {Sobrenome}";
    }

    // Classe Suite - representa o quarto
    public class Suite
    {
        public string TipoSuite { get; set; }
        public int Capacidade { get; set; }
        public decimal ValorDiaria { get; set; }

        public Suite(string tipoSuite, int capacidade, decimal valorDiaria)
        {
            TipoSuite = tipoSuite;
            Capacidade = capacidade;
            ValorDiaria = valorDiaria;
        }
    }

    // Classe Reserva - relaciona hóspedes e suíte
    public class Reserva
    {
        public List<Pessoa> Hospedes { get; set; }
        public Suite Suite { get; set; }
        public int DiasReservados { get; set; }

        public Reserva(int diasReservados)
        {
            DiasReservados = diasReservados;
            Hospedes = new List<Pessoa>();
        }

        // Cadastra os hóspedes
        public void CadastrarHospedes(List<Pessoa> hospedes)
        {
            if (hospedes.Count <= Suite.Capacidade)
            {
                Hospedes = hospedes;
            }
            else
            {
                throw new Exception("Capacidade da suíte é menor que o número de hóspedes.");
            }
        }

        // Cadastra a suíte
        public void CadastrarSuite(Suite suite)
        {
            Suite = suite;
        }

        // Retorna quantidade de hóspedes
        public int ObterQuantidadeHospedes()
        {
            return Hospedes.Count;
        }

        // Calcula valor total da diária
        public decimal CalcularValorDiaria()
        {
            decimal valor = DiasReservados * Suite.ValorDiaria;

            // Aplica desconto de 10% se mais de 10 dias
            if (DiasReservados > 10)
            {
                valor *= 0.9m;
            }

            return valor;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Criando a suíte
            Suite suiteLuxo = new Suite("Luxo", 3, 150);

            // Criando reserva de 12 dias
            Reserva reserva = new Reserva(12);
            reserva.CadastrarSuite(suiteLuxo);

            // Criando hóspedes
            List<Pessoa> hospedes = new List<Pessoa>
            {
                new Pessoa("Mariano", "Silva"),
                new Pessoa("João", "Pereira")
            };

            reserva.CadastrarHospedes(hospedes);

            // Exibindo resultados
            Console.WriteLine($"Hóspedes: {reserva.ObterQuantidadeHospedes()}");
            foreach (var hospede in reserva.Hospedes)
            {
                Console.WriteLine($"- {hospede.NomeCompleto}");
            }

            Console.WriteLine($"Suíte: {reserva.Suite.TipoSuite}");
            Console.WriteLine($"Dias reservados: {reserva.DiasReservados}");
            Console.WriteLine($"Valor total: R$ {reserva.CalcularValorDiaria()}");
        }
    }
}
